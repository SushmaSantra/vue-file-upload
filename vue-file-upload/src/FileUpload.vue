<template>
  <div>
    <div class="col-12">
      <input 
        type="file"
        v-bind:class="input_class"
        @change="onChangeFile"

       />
    </div>
  </div>
</template>




<script>
export default {
  name: "FileUpload",
  props: { "input_class",  },
  data() {
    return {
        file: null,
    }
  },
  methods : {
    onChangeFile(e){
      const files = e.target.files || e.dataTransfer.files;
      if (!files.length) {
        return;
      }
      const file = files[0];
      const size = file.size && (file.size / Math.pow(1024, 2));
      // check file max size
      if (size > this.maxSize) {
        this.$emit('maxSizeLimit', size);
        this.file = null;
        return;
      }
      // update file
      this.file = file;
      this.$emit('fileReady', file);
      const reader = new FileReader();
      reader.onload = e => {
        const dataURI = e.target.result;

        if (dataURI) {
          this.$emit('fileLoaded', dataURI);

          this.preview = dataURI;
        }
      }//end onload
      // read blob url from file data
      reader.readAsDataURL(file);
    },

    



  },
};
</script>