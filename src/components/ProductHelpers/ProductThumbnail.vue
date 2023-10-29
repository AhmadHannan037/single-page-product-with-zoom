<!-- 
System: Single Product page with zoom
Organization: XYZ
Developer:Ahmad Hanan
Date: 10/29/2023
Purpose: This component display the product thumbnail to user
-->
<template>
  <!-- Thumbnail Component -->
  <div class="flex justify-center mt-4">
    <div
      v-for="media in thumbnailImages"
      :key="media.id"
      @click="selectImage(media.id)"
      :class="{
        'border-2 border-slate-400': selectedImage === indexMap[media.id],
      }"
      class="thumbnail-image mx-2 p-2 cursor-pointer"
    >
      <div class="media-container w-9 h-12 overflow-hidden">
        <video
          v-if="media.type === 'video'"
          :src="media.url"
          class="media-content w-full h-full object-cover"
          :alt="media.alt"
        ></video>
        <img
          v-else
          :src="media.url"
          class="media-content w-full h-full object-cover"
          :alt="media.alt"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    thumbnailImages: Array, // Array of thumbnail images as a prop
    sliderImages: Array, // Array of all slider images as a prop
    selectedImage: Number, // Index of the selected image as a prop
  },
  computed: {
    /**
     * Return the map of media id with index, utilizing single array for images/videos
     *
     * @param {void}
     * @return {map} sliderimage Map
     */
    indexMap() {
      // Create a mapping of media id to index in the sliderImages array
      const map = {};
      this.sliderImages.forEach((media, index) => {
        map[media.id] = index;
      });
      return map;
    },
  },
  methods: {
    /**
     * Find out the index base on media id and then emit the parent
     *
     * @param {Number} mediaId
     * @return {void} none
     */
    selectImage(mediaId) {
      // Find the index of the selected media in sliderImages
      const index = this.sliderImages.findIndex(
        (media) => media.id === mediaId
      );
      if (index !== -1) {
        // Emit an event to update the selected image
        this.$emit("update:selectedImage", index);
      }
    },
  },
};
</script>
