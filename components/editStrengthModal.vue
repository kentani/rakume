<template>
  <v-card>
    <v-card-title>
      <span class="headline">Strength</span>
    </v-card-title>
    <v-card-text class="pb-0">
      <v-combobox
        v-model="editStrengthList"
        multiple
        clearable
        chips
        :items="editStrengthList"
        :color="color"
        :item-color="color"
        label="強み"
        placeholder="強みを入力"
      />
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn text @click="closeStrengthModal">close</v-btn>
      <v-btn text @click="saveStrengthModal">save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      editStrengthList: this.strengthList
    }
  },
  props: {
    strengthList: {
      type: Array,
      require: true
    },
    color: {
      type: String,
      default: 'dark'
    },
  },
  methods: {
    closeStrengthModal() {
      this.editStrengthList = this.strengthList
      this.$emit("handleEditStrengthClosed", this.editStrengthList)
    },
    saveStrengthModal() {
      this.$emit("handleEditStrengthSaved", this.editStrengthList)
    }
  }
}
</script>