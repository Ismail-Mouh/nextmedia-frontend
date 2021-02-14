<template>
  <div>
    <input type="file" accept=".jpeg,.png" @change="uploadImage">
  </div>
</template>

<script>

export default {
  data() {
    return {
    }
  },

  methods : {
    uploadImage(e) {
      let files = e.target.files || e.dataTransfer.files;
      if (!files.length)
        return;
      let reader = new FileReader();
      reader.onload = (e) => {
        this.$emit('updateImage',e.target.result)
      }
      reader.readAsDataURL(files[0])
    },
  }

}
</script>