<template>
  <v-card>
    <v-card-title>
      <span class="headline">Hobby</span>
    </v-card-title>
    <v-card-text class="pb-0">
      <v-textarea
        v-model="editHobby"
        filled
        label="趣味"
        placeholder="趣味を入力"
        :color="color" />
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn rounded text @click="closeHobbyModal">close</v-btn>
      <v-btn rounded text @click="saveHobbyModal">save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      editHobby: this.hobby
    }
  },
  props: {
    hobby: {
      type: String,
      require: true
    },
    color: {
      type: String,
      default: 'dark'
    },
  },
  methods: {
    closeHobbyModal() {
      this.editHobby = this.hobby
      this.$emit("handleEditHobbyClosed", this.editHobby)
    },
    saveHobbyModal() {
      this.$emit("handleEditHobbySaved", this.editHobby)
    }
  }
}
</script>