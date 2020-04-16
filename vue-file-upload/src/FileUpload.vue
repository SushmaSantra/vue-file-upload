<template>
  <div>
    <sub-file-upload 
        :input_id_s="id"
        :key="UpdateCounter"
        :forceUpdateCounterS="forceUpdateCounter"
        :multiple_s="multiple_file_upload"
        :input_class_s="input_class"
        :preview_s="preview_file"
        :label_s="label_input"
        :label_class_s="input_class"
        :base_s="base_val"
        :max_size_s="max_file_size"
        :max_select_s="max_select_file"
        @forceRerenderFromSubC = "forceRerender"
        @onMaxSize_s = "maxSizeLimit"
        @onFileLoaded_s = "onfileLoaded"
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
  props: [ "input_class" , "multiple_file_upload" ,"preview_file" , "label_input" , "base_val" , "max_file_size" , "id" , "label_class" ,"max_select_file", "UpdateCounter"],
  data() {
    return {
      forceUpdateCounter:0,
    }
  },
  methods : {
        forceRerender(val, file) {
          this.forceUpdateCounter += 1;  
        },
        maxSizeLimit(val) {
          this.$emit('maxSizeLimit',val)
        },
        onfileLoaded(val) {
          this.$emit('fileLoaded',val)
        }

  },
};
</script>
