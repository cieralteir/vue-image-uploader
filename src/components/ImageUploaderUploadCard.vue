<template>
  <div :class="outerClass" v-show="show">
    <div class="card caiu__card">
      <vue-element-loading color="#555" :active="loading"></vue-element-loading>
      <img
        class="card-img-top caiu__card__image"
        :id="`caiu__card__image-${index}`"
        :src="image.thumbnail"
      />
      <div class="caiu__card__overlay">
        <div class="caiu__card__content--center">{{ image.name }}</div>
        <div class="btn-group caiu__card__content--right">
          <button class="btn btn-sm btn-dark" @click.prevent="remove()">
            <i class="fa fa-trash"></i>
          </button>
          <button class="btn btn-sm btn-dark" @click.prevent="showImage = true">
            <i class="fa fa-search"></i>
          </button>
        </div>
      </div>
    </div>
    <image-uploader-image-preview :image="image" @close="showImage = false" v-if="showImage" />
  </div>
</template>

<script>
import axios from "axios";
import VueElementLoading from "vue-element-loading";
import ImageUploaderImagePreview from "./ImageUploaderImagePreview";

export default {
  props: {
    image: {
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    outerClass: {
      type: String,
      default: "col-sm-6 col-lg-4 col-xl-2"
    }
  },
  components: {
    VueElementLoading,
    ImageUploaderImagePreview
  },
  data: () => ({
    show: true,
    showImage: false,
    loading: false
  }),
  methods: {
    remove() {
      let confirmed = confirm("Are you sure you want to remove this image?");

      if (confirmed) {
        this.loading = true;

        axios.delete(this.image.deleteUrl).then(
          () => {
            this.loading = false;
            this.show = false;
            this.$emit("removed", this.index);
          },
          () => {
            if (this.$toasted) {
              this.$toasted.error(`Failed to delete ${this.image.name}.`, {
                icon: { name: "fa-check" }
              });
            } else {
              alert(`Failed to delete ${this.image.name}.`);
            }
            this.loading = false;
          }
        );
      }
    }
  }
};
</script>
