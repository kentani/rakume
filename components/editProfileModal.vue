<template>
  <v-card>
    <v-card-title>
      <span class="headline">Profile</span>
    </v-card-title>
    <v-card-text class="pb-0">
      <v-textarea
        v-model="editProfile"
        filled
        label="プロフィール"
        placeholder="プロフィールを入力"
        :color="color" />
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn rounded text @click="closeProfileModal">close</v-btn>
      <v-btn rounded text @click="saveProfileModal">save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      editProfile: this.profile
    }
  },
  props: {
    profile: {
      type: String,
      require: true
    },
    color: {
      type: String,
      default: 'dark'
    },
  },
  methods: {
    closeProfileModal() {
      this.editProfile = this.profile
      this.$emit("handleEditProfileClosed", this.editProfile)
    },
    saveProfileModal() {
      this.$emit("handleEditProfileSaved", this.editProfile)
    }
  }
}
</script>