<template>
  <div class="wrapper">
    <h1>Photo grid</h1>

    <div class="row">
      <form>
        <div class="form-row">
          <div class="form-group">
            <label for="inputModel">Camera model</label>
            <select id="inputModel" class="form-control" v-model="selected_model" debounce="500">
              <option v-for="model in model_list" :key="model" :value="model">
                {{ model }}
              </option>
            </select>
          </div>
          <div class="form-group">
            <label for="inputLens">Lens</label>
            <select id="inputLens" class="form-control" v-model="selected_lens" debounce="500">
              <option v-for="lens in lens_list" :key="lens" :value="lens">
                {{ lens }}
              </option>
            </select>
          </div>

          <!--          <div class="form-group">-->
          <!--            <label for="inputShutter">Shutter speed</label>-->
          <!--            <input id="inputShutter" type="range" class="custom-range" min="0" max="5">-->
          <!--          </div>-->


        </div>
      </form>

    </div>


    <div class="row">
      <div class="col-lg-4 col-md-4 col-sm-12" v-for="image in currentImages.slice(0, 160)" :key="image.largest_thumb">
        <a href="#" class="d-block mb-4 h-100 chocolat-image" :title="image.name">
          <img class="img-fluid img-thumbnail"
               :src="image.smallest_thumb"
               :srcset="image.srcset"
               sizes="
                          (max-width: 1200px) 100vw,
                          (max-width: 1850px) 50vw,
                          (max-width: 4000px) 25vw,
                          25vw"
               :alt="image.name">
        </a>
      </div>

      <h1>and {{ Math.max(currentImages.length - 160, 0) }} more photos</h1>
    </div>

  </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: 'PhotoGrid',
        data: function () {
            return {
                photos: [],
                model_list: [],
                lens_list: [],
                selected_model: null,
                selected_lens: null,
                selected_shutter: 1
            }
        },

        mounted() {
            this.fetch_data();
        },
        computed: {
            currentImages: function () {
                var photos = this.photos
                if (this.selected_model != null && this.selected_model !== "") {
                    photos = photos.filter(f => f.tags.model === this.selected_model)
                }
                if (this.selected_lens != null && this.selected_lens !== "") {
                    photos = photos.filter(f => f.tags.lens === this.selected_lens)
                }

                return photos;
            }
        },
        methods: {
            fetch_data: function () {
                axios.get(
                    '/pictures.json'
                ).then(
                    r => {
                        this.photos = r.data.pics;
                        this.model_list = r.data.models;
                        this.lens_list = r.data.lens;
                    }
                ).catch(
                    error => {
                        console.log("Failed to fetch data!", error);
                    }
                );

            }
        }


    }
</script>