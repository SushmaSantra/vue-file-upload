## vue-file-upload-as-bsf (base64)

### Install using npm
```
npm i vue-file-upload-as-bsf
```

### Features

```
  1. Multiple File upload
  2. File preview option
  3. Function to handle, exceed in file size
  4. Maximum file size ( options available, base 10 or base 2)
  5. Custom css class for Input component
  6. Input Disable option
  7. Update Counter 
      Used to render the component, from a simple counter. In simple terms, to clear the Input value.

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
      :is_disable="isDisable"
      :UpdateCounter="updateCounter"
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
      ImageMaxSize: 10, //(size of file in mb, default value 10)
      canPreView: true, //boolean value to preview the file
      baseValue: 2, // 2 or 10 (1000 or 1024 conversion)
      filesUploaded: [],
      isDisable: true, //You can disable the input label
      updateCounter: 0 //You can increment this counter if you want to re-render your component
    }
  },
  methods: {
    thumbUrl (file) {
      return file.myThumbUrlProperty
    },
    onFileChange (file) {
      // Handle files like:
      this.filesUploaded = file
    },
    onSizeExceeded (file) {
      //You can show an error message here, with file name and size
    }
  } 
}
</script>
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
