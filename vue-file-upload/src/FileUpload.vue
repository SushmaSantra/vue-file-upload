<script>
import { setTimeout } from "timers";
export default {
  name: "FileUpload",
  props: {
    message: {
      type: String,
      default: "Hello Dev.to"
    },
    timeout: {
      type: Number,
      default: 0
    }
  },
  mounted() {
    setTimeout(() => {
      alert(this.message);
    }, this.timeout);
  }
};
</script>