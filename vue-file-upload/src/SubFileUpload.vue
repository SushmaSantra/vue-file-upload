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
	       <label id="inputLabelCSS" :title="label_input" :class="label_class_s" :for="input_id_s">{{label_input}}</label>
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
			forceUpdateCounterS : Number,
			forceUpdateU : {
				type:Number,
				default:1
			},
			max_select_s : {
				type: Number,
				default: 10
			}

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
		
		created : function(){
			if(this.label_s != null || this.label_s !='')
			{
				this.label_input = this.label_s;
			}

		},
		methods : {
			readAndPreview(file, index, arr){
		        var allFilest = [];
		        var size;
		        const reader = new FileReader();
				
				size = file.size && (file.size / Math.pow(this.base_size, 2));
				  
		      	// check file max size and force re-render the component, so that the variables are initialized to defaults.
			      if (size > this.max_size_s || forceUpdateU!=1) {
					console.log("Max size")
			      	// trigger the event , when the size is exceeded the given val
					this.$emit('onMaxSize_s', file);

					// trigger the component key, to re-render the component
					this.$emit('forceRerenderFromSubC', this.forceUpdateCounterS, file);

			        
					return;
					
			      }
				  
				  allFilest[0] = file;

				  this.allFiles.push(file);
				
				  
				  reader.onload = e => {

					const dataURI = e.target.result;
					
			        if (dataURI) {

					  this.label_input = file.name;
					  allFilest[0].data = dataURI;

					  this.$emit('onFileLoaded_s', allFilest);
					  
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
		      if (files.length > this.max_select_s) {
		      	this.allFiles = [];
		      	this.showSnackbar();
		      	return;
		      }
		      if(files) {
		      	[].forEach.call(files, this.readAndPreview);
		      }
		      
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

	#inputLabelCSS {
		color: darkslategray; 
		text-overflow: ellipsis;
		overflow: hidden;
		white-space: nowrap;
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