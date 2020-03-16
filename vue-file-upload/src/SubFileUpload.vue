<template>
	<div class="row">
		<div class="col-12 hc">
	      <input 
		    type="file"
	      	:id="input_id_s" 
	      	:multiple="multiple_s" 
	        :class="input_class_s"
	        @change="onChangeFile"
	       />
	       <label :class="label_class_s" :for="input_id_s">{{label_input}}</label>
	    </div>
        <div class="col-12" v-if="preview_s">
	        <img 
	          :src="preview"
	          class="img-fluid" 
	        />
	    </div>
	</div>
</template>

<script>
	export default {
		props : {
			input_id_s : {
				type: String,
				default: "fileuploadInput"
			},
			multiple_s : {
				type: Boolean,
				default: false
			},
			input_class_s : {
				type: String,
				default: ''
			},
			label_class_s : {
				type: String,
				default: ''				
			},
			preview_s : {
				type: Boolean,
				default: false
			},
			label_s : {
				type: String,
				default: "Select a File"
			},
			base_s : {
				type: Number,
				default: 2
			},
			max_size_s : {
				type: Number,
				default: 15
			},
			forceUpdateCounterS : Number

		},
		data () {
			return {
				file : null,
				label_input: '',
				base_size: 1024,
				preview: ''
			}
		},
		// watch : {
		// 	label_s : function (val) 
		// 	{
		// 		if(val != null)
		// 		{
		// 			this.label_input = val;
		// 		}
		// 	},

		// 	base_s : function (val)
		// 	{
		// 		if (val == 10)
		// 		{
		// 			this.base_size = 1000
		// 		}
		// 		else
		// 		{
		// 			this.base_size = 1024
		// 		}
				 
		// 	}
		// },
		mounted : function(){
			if(label_s != null || label_s !='')
			{
				this.label_input = label_s;
			}

			if (base_s == 10)
			{
				this.base_size = 1000
			}
			else
			{
				console.log("Enter a valid Base value, i.e 10 or 2. Since, no valid input was given the base size will be set to its default value (1024). ")
				this.base_size = 1024
			}

		},
		methods : {
			onChangeFile(e){
		      const files = e.target.files || e.dataTransfer.files;
		      if (!files.length) {
		        return;
		      }
		      const file, allFiles;
		      const size;
		      const reader = new FileReader();

		      for( var i = 0; i < this.files.length; i++ ) {
		      	file = files[i];
		      	size = file.size && (file.size / Math.pow(this.base_size, 2));
		      	// check file max size and force re-render the component, so that the variables are initialized to defaults.
			      if (size > this.maxSize) {
			      	// trigger the event , when the size is exceeded the given val
			        this.$emit('onMaxSize_s', file);
			        // trigger the component key, to re-render the component
			        this.$emit('forceRerenderFromSubC', forceUpdateCounterS);
			        continue;
			      }
		      	allFiles.push(file);
		      	reader.onload = e => {
			        const dataURI = e.target.result;

			        if (dataURI) {
			          this.$emit('onFileLoaded_s', dataURI);
			          this.preview = dataURI;
			        }
			    }//end onload
			    // read blob url from file data
			      reader.readAsDataURL(file);
		      }	      

		      // update file
		      // this.file = file;
		      this.$emit('onFileReady_s', allFiles);

		      
		      

		      

		    },
		},
	}
</script>

<style>
	.hc > input[type="file"] {
		display: none;
	}
</style>