<template class="flex flex-col flex-grow">
  <div class="flex flex-col flex-grow overflow-auto justify-end spac0 ">
    <EditorContent :editor="editor" class="mb-auto "/>


    <div class="justify-between border-t border-gray-300 bg-gray-100 text-right px-16 py-8">

      <button @click="saveNote" class="border border-black px-4 rounded-lg hover:bg-black hover:text-white hover:border-white " >Save Notes</button>
    </div>


  </div>




</template>

<script>


import { Editor, EditorContent} from "@tiptap/vue-3";
import StarterKit from '@tiptap/starter-kit'


export default {
  name: "main-content",
  components: {EditorContent, },
  data() {
    return {
      editor: null,
      datebase: null,
    }
  },
  async created() {
    this.datebase = await this.getDatabase();
  },

  mounted() {
    this.editor = new Editor({
      content: '',
      extensions:[
        StarterKit
      ],
      editorProps: {
        attributes:  {
          class: "prose my-6 mx-auto focus:outline-none"
        }
      }

    });
  },

  beforeUnmount() {
    this.editor.destroy();
  },

  methods: {
   async getDatabase() {
     return new Promise((resolve, reject) => {
       let db = window.indexedDB.open("notes");

       db.onerror = e => {
         console.log(e)
         reject('Error opening the database');


       };

       db.onsuccess = e => {

         console.log('db.onsuccess', e);
         resolve('e.target.result');


       };

       db.onupgradeneeded = e => {
         console.log('db.oneupgradeneeded', e);
         e.target.result.createObjectStore("notes");
       };
     });
   },
    async saveNote() {
      // eslint-disable-next-line
     return new Promise((resolve, reject) => {
       let transaction = this.database.transaction('notes', 'readwrite');
       // eslint-disable-next-line
       transaction.oncomplete = e => {
         resolve();
       }

       let now = new Date();
       transaction.objectStore('notes').add({
         content: this.editor.getHTML(),
         created: now.getTime()
       });


     });
    }

  }

}
</script>

<style scoped>

</style>