<template>
  <v-card>
    <v-card-title>
      <span class="headline">Name</span>
    </v-card-title>
    <v-card-text class="pb-0">
      <v-text-field
        v-model="editName"
        filled
        label="名前"
        placeholder="名前を入力"
        :color="color" />
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn rounded text @click="closeNameModal">close</v-btn>
      <v-btn rounded text @click="saveNameModal">save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      editName: this.name
    }
  },
  props: {
    name: {
      type: String,
      require: true
    },
    color: {
      type: String,
      default: 'dark'
    },
  },
  methods: {
    closeNameModal() {
      this.editName = this.name
      this.$emit("handleEditNameClosed", this.editName)
    },
    saveNameModal() {
      this.$emit("handleEditNameSaved", this.editName)
    }
  }
}
</script>