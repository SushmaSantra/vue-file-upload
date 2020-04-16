## vue-file-upload-as-bsf (base64)

### Install using npm
```
npm i vue-file-upload-as-bsf
```

### Basic Usage

```
import Vue from 'vue'
 
import FileUploadAsBSF from 'vue-file-upload-as-bsf'
 
Vue.use(FileUploadAsBSF)

```


```
<template>
	<vue-file-upload-as-bsf
	    id="id1"
	    class="classname1 classname2 .."
	    input_class="inputclass1 inputclass2 .."
	    :max_file_size="ImageMaxSize"
	    :preview_file="canPreView"
	    :base_val="baseValue"
	    @maxSizeLimit="onSizeExceeded"
	    @fileLoaded="onLoad"
	/>
</template>



<script>
import Vue from 'vue'
import FileUploadAsBSF from 'vue-file-upload-as-bsf'
Vue.use(FileUploadAsBSF)
 
export default {
  data () {
    return {
      ImageMaxSize: 10, (size of file in mb, default value 10)
      canPreView: true/false, (to preview the file)
      baseValue: 2/10, (1000 or 1024 conversion)
      filesUploaded: []
    }
  },
  methods: {
    thumbUrl (file) {
      return file.myThumbUrlProperty
    },
    onFileChange (file) {
      // Handle files like:
      this.fileUploaded = file
    }
  } 
}
</script>
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
