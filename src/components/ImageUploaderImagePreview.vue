<template>
  <div class="modal caiu__preview" tabindex="-1">
    <div class="modal-dialog modal-lg" role="document">
      <div class="modal-content caiu__preview__content">
        <div class="modal-header caiu__preview__content__header">
          <h5 class="modal-title">Preview</h5>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-12 text-center">
              <img :src="image.source" class="caiu__preview__content__image" />
            </div>
          </div>
        </div>
        <div class="modal-footer caiu__preview__content__footer">
          <button
            type="button"
            class="btn btn-sm btn-secondary"
            @click.prevent="$emit('close')"
          >Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    image: {
      type: Object,
      required: true
    }
  }
};
</script>
