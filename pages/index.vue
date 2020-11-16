<template>
  <v-row>
    <v-col cols="0" md="3" sm="0"> </v-col>

    <v-col cols="12" md="6" sm="12">
      <v-card max-width="100%" elevation="5">
        <v-card-text>
          <div class="mx-auto text-center">
            <v-avatar class="mt-2" color="#5c2684">
              <v-icon size="70" dark> mdi-account-circle </v-icon>
            </v-avatar>
          </div>
        </v-card-text>
        <v-divider></v-divider>
        <v-list-item-content class="justify-center">
          <h3
            class="text-capitalise font-weight-black pt-5 primary--text text-center"
          >
            Create text
          </h3>
          <!--  -->
          <v-form
            @submit.prevent="onPost"
            ref="form"
            class="mt-1 mb-6 pr-8 pl-8 pb-8 pt-4"
            v-model="valid"
            lazy-validation
          >
            <v-file-input
              outlined
              accept="image/*"
              dense
              v-model="image"
              append-icon="mdi-camera"
              error-count="2"
              type="file"
              :rules="fileRules"
              label="Picture"
              color="#5c2684"
              required
            ></v-file-input>

            <v-textarea v-model="decription"> </v-textarea>

            <v-btn
              shaped
              block
              large
              type="submit"
              color="#5c2684"
              class="mr-4 white--text"
              @click="validate"
            >
              Create Text
              <span style="display: none" class="custom-loader">
                <v-icon light>cached</v-icon>
              </span>
            </v-btn>
          </v-form>
        </v-list-item-content>
      </v-card>
    </v-col>

    <v-col cols="0" md="3" sm="0"> </v-col>
  </v-row>
</template>


<script>
const url = 'https://shamskhalil.ngrok.io/tika/form'
const proxyurl = 'https://cors-anywhere.herokuapp.com/'

export default {
  data() {
    return {
      valid: true,
      image: null,
      file: '',
      decription: '',
      fileRules: [(v) => !!v || 'File is required'],
    }
  },

  methods: {
    validate() {
      this.$refs.form.validate()
    },

    onPost() {
      //    return console.log(this.image)
      const formData = new FormData()
      formData.append('image', this.image)
      return this.$axios
        .post(`${proxyurl}${url}`, formData, {
          headers: {
            Accept: 'text/plain',
          },
        })
        .then((userData) => {
          // console.log(userData)
          // console.log(userData['data'])
          this.decription = userData['data']
        })
        .catch((err) => {
          console.log(err)
        })
        .catch(({ response }) => {
          console.log(response)
        })
    },
  },

  created() {},
  head: {
    title: 'Add a Post',
    meta: [
      {
        hid: 'description',
        name: 'description',
        content: '',
      },
    ],
  },
}
</script>
