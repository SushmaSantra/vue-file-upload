<template>
  <div>
    <sub-file-upload 
        :input_id_s="id"
        :key="forceUpdateCounter"
        :forceUpdateCounterS="forceUpdateCounter"
        :multiple_s="multiple_file_upload"
        :input_class_s="input_class"
        :preview_s="preview_file"
        :label_s="label_input"
        :label_class_s="label_class"
        :base_s="base_val"
        :max_size_s="max_file_size"
        @forceRerenderFromSubC = "forceRerender"
        @onFileReady_s = "fileReady"
        @onMaxSize_s = "maxSizeLimit"
        @onFileLoaded_s = "fileLoaded"
    />
  </div>
</template>




<script>
import SubFileUpload from "./SubFileUpload.vue"

export default {

  name: "FileUpload",
  components : {
    "sub-file-upload" : SubFileUpload
  },
  props: [ "input_class" , "multiple_file_upload" ,"preview_file" , "label_input" , "base_val" , "max_file_size" , "id" , "label_class" ],
  data() {
    return {
      forceUpdateCounter:0,
    }
  },
  methods : {
        forceRerender(val) {
          this.forceUpdateCounter += 1;  
        },
        fileReady(val) {
          this.$emit()
        },
        maxSizeLimit(val) {

        },
        fileLoaded(val) {

        }

  },
};
</script>