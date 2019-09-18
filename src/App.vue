<template>
  <div class="row caiu">
    <div class="col-12">
      <image-uploader-file-input
        :input-name="inputName"
        :multiple="multiple"
        :accepted-mime-types="acceptedMimeTypes"
        @changed="onFileChanged"
        ref="fileInput"
      ></image-uploader-file-input>
    </div>
    <div class="col-12">
      <hr />
      <div class="caiu___container">
        <transition-group
          name="fade"
          tag="div"
          class="row"
          v-show="imagesCount"
        >
          <image-uploader-image-card
            :image="image"
            :outer-class="cardClass"
            :upload-on-create="apiMode"
            :upload-url="uploadUrl"
            :input-name="inputName"
            @uploaded="onImageUploaded"
            v-for="(image, index) in images"
            :key="'image-' + index"
          ></image-uploader-image-card>
        </transition-group>
        <transition-group
          name="fade"
          tag="div"
          class="row"
          v-show="uploadsCount"
        >
          <image-uploader-upload-card
            :image="upload"
            :outer-class="cardClass"
            :index="index"
            @removed="onImageRemoved"
            v-for="(upload, index) in uploads_"
            :key="'upload-' + index"
          ></image-uploader-upload-card>
        </transition-group>
        <div v-if="!imagesCount && !uploadsCount">No image/s.</div>
      </div>
    </div>
  </div>
</template>

<script>
import ImageUploaderFileInput from "./components/ImageUploaderFileInput";
import ImageUploaderImageCard from "./components/ImageUploaderImageCard";
import ImageUploaderUploadCard from "./components/ImageUploaderUploadCard";

export default {
  props: {
    apiMode: {
      type: Boolean,
      default: false
    },
    uploadUrl: {
      type: String
    },
    uploads: {
      type: Array,
      default: () => []
    },
    multiple: {
      type: Boolean,
      default: false
    },
    inputName: {
      type: String,
      default: "image"
    },
    cardClass: {
      type: String,
      default: "col-sm-6 col-lg-4 col-xl-2"
    },
    acceptedMimeTypes: {
      type: Array,
      default: function() {
        return ["image/jpeg", "image/png"];
      }
    },
    maxFileSize: {
      type: Number,
      default: 3
    }
  },
  components: {
    ImageUploaderFileInput,
    ImageUploaderImageCard,
    ImageUploaderUploadCard
  },
  data: () => ({
    images: [],
    uploads_: [],
    imagesCount: 0,
    uploadsCount: 0
  }),
  mounted() {
    this.uploads_ = this.uploads;
    this.uploadsCount = this.uploads_.length;
  },
  methods: {
    onFileChanged(files) {
      if (!this.multiple || !this.apiMode) {
        this.images = [];
        this.imagesCount = 0;
      }

      this.$nextTick(() => {
        for (let x = 0; x < files.length; x++) {
          this.images.push(files[x]);
          this.imagesCount++;
        }

        if (this.apiMode) {
          this.$refs.fileInput.clear();
        }

        this.$emit("file-changed", files);
      });
    },
    onImageUploaded(image) {
      if (!this.multiple) this.uploads_ = [];

      this.$nextTick(() => {
        this.uploads_.push(image);
        this.imagesCount--;
        this.uploadsCount++;
      });
    },
    onImageRemoved() {
      this.uploadsCount--;
    }
  }
};
</script>

<style lang="scss">
.caiu {
  .caiu__card {
    margin: 5px 0px;
  }
  .caiu___container {
    background: #f1f1f1;
    padding: 15px;
    max-height: 400px;
    overflow-y: scroll;
  }
  .caiu__card__image {
    height: 200px;
    object-fit: cover;
  }
  .caiu__card__overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.8);
    border-radius: 0.25rem;
  }
  .caiu__card__overlay:hover {
    opacity: 1;
  }
  .caiu__card__overlay {
    opacity: 0;
  }
  .caiu__card__content--center {
    position: absolute;
    top: 50%;
    transform: translateY(-40%);
    text-align: center;
    color: #eee;
    width: 100%;
    padding: 0px 10px;
  }
  .caiu__card__content--left {
    position: absolute;
    bottom: 5px;
    left: 5px;
  }
  .caiu__card__content--right {
    position: absolute;
    bottom: 5px;
    right: 5px;
  }
  .modal {
    display: block;
    background: rgba(0, 0, 0, 0.8);
  }
  .caiu__preview .modal-dialog {
    max-width: 100%;
  }
  .caiu__preview__content {
    background: transparent;
    border: none;
  }
  .caiu__preview__content__header {
    border-bottom: none;
    display: block;
    text-align: center;

    .modal-title {
      color: whitesmoke;
      font-weight: bold;
    }
  }
  .caiu__preview__content__image {
    max-height: 500px;
    max-width: 100%;
  }
  .caiu__preview__content__footer {
    border-top: none;
    display: block;
    text-align: center;

    button {
      background: transparent;
      color: whitesmoke;
    }
    button:hover {
      background: whitesmoke;
      color: #222;
    }
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.05s linear;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
}
</style>
