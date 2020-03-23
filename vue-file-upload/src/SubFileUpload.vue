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
	       <label id="inputLabelCSS" :class="label_class_s" :for="input_id_s">{{label_input}}</label>
	    </div>
	    <div id="snackbar">You cannot select more than {{ }}5 files.</div>
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
				preview: '',
				allFiles : []
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
		created : function(){
			if(this.label_s != null || this.label_s !='')
			{
				this.label_input = this.label_s;
			}

			if (this.base_s == 10)
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
			readAndPreview(file, index, arr){
		        var allFiles = [];
		        var size;
		        const reader = new FileReader();
		        console.log(index)
		      	size = file.size && (file.size / Math.pow(this.base_size, 2));
		      	// check file max size and force re-render the component, so that the variables are initialized to defaults.
			      if (size > this.max_size_s) {
			      	console.log("Max size")
			      	// trigger the event , when the size is exceeded the given val
			        this.$emit('onMaxSize_s', file);
			        // trigger the component key, to re-render the component
			        this.$emit('forceRerenderFromSubC', this.forceUpdateCounterS, file);
			        this.allFiles.push({});
			        return;
			      }
			    console.log(allFiles)
		      	this.allFiles.push(file);

		      	reader.onload = e => {
			        const dataURI = e.target.result;

			        if (dataURI) {
			          this.label_input = file.name;
			          this.$emit('onFileLoaded_s', dataURI);
			          this.preview = dataURI;
			          this.allFiles[index].data = dataURI;
			        }
			    }//end onload
			    // read blob url from file data
			    reader.readAsDataURL(file);

			},
			onChangeFile(e){
				this.allFiles = [];
		      const files = e.target.files || e.dataTransfer.files;
		      if (!files.length) {
		        return;
		      }
		      if (files.length>5) {
		      	this.allFiles = [];
		      	this.showSnackbar();
		      	return;
		      }
		      // var file;
		      // var allFiles = [];
		      // var size;
		      // const reader = new FileReader();

		      if(files) {
		      	[].forEach.call(files, this.readAndPreview);
		      }

		     //  for( var i = 0; i < files.length; i++ ) {
		     //  	file = files[i];
		     //  	size = file.size && (file.size / Math.pow(this.base_size, 2));
		     //  	// check file max size and force re-render the component, so that the variables are initialized to defaults.
			    //   if (size > this.max_size_s) {
			    //   	console.log("Max size")
			    //   	// trigger the event , when the size is exceeded the given val
			    //     this.$emit('onMaxSize_s', file);
			    //     // trigger the component key, to re-render the component
			    //     this.$emit('forceRerenderFromSubC', this.forceUpdateCounterS, file);
			    //     continue;
			    //   }
			    // console.log(allFiles)
		     //  	allFiles.push(file);

		     //  	reader.onload = e => {
			    //     const dataURI = e.target.result;

			    //     if (dataURI) {
			    //       this.label_input = file.name;
			    //       this.$emit('onFileLoaded_s', dataURI);
			    //       console.log(dataURI)
			    //       this.preview = dataURI;
			    //     }
			    // }//end onload
			    // // read blob url from file data
			    // reader.readAsDataURL(file);
			      
		     //  }	      

		      // update file
		      // this.file = file;
		      console.log("TADAAAAA..........................................")
		      console.log(this.allFiles)
		      this.$emit('onFileReady_s', this.allFiles);

		    },
		    showSnackbar() {
		    	var x = document.getElementById("snackbar");
			    x.className = "show";
			    setTimeout(function(){ x.className = x.className.replace("show", ""); }, 4000);
		    },
		},
	}
</script>

<style>
	.hc > input[type="file"] {
		display: none;
	}
	.hc {
		border-width: 0 0 1px 0;
		border-style: solid;
		border-color: grey;
	}
	#inputLabelCSS {
		color: darkslategray; 
	}
	#snackbar {
	  	visibility: hidden;
	    min-width: 250px;
	    margin-left: -125px;
	    background-color: rgba(21, 21, 21, 0.37);
	    color: #fff;
	    text-align: center;
	    border-radius: 2px;
	    padding: 10px;
	    position: fixed;
	    z-index: 1;
	    left: 50%;
	    bottom: 30px;
	    font-size: 17px;
	}

	#snackbar.show {
	  visibility: visible;
	  -webkit-animation: fadein 0.5s, fadeout 0.5s 2.5s;
	  animation: fadein 0.5s, fadeout 0.5s 2.5s;
	}
</style>