<template>
  <v-card>
    <v-card-title>
      <span class="headline">Skills</span>
    </v-card-title>
    <v-card-text class="pb-0">
      <v-card-actions
        class="body-2 grey--text text--darken-1">
        <v-text-field
          v-model="skillItem"
          label="スキル名"
          placeholder="スキル名を入力"
          :color="color"
        />
        <v-spacer />
        <v-rating
          v-model="skillRating"
          background-color="grey"
          :color="color"
          dense
        />
      </v-card-actions>
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn text @click="closeSkillModal">close</v-btn>
      <v-btn text @click="saveSkillModal">save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    skill: {
      type: Object,
      require: true
    },
    color: {
      type: String,
      default: 'dark'
    },
  },
  computed: {
    skill2() {
      return this.skill 
    },
    skillItem: {
      get: function () {
        return this.skill2.item
      },
      set: function (newValue) {
        this.skill2.item = newValue
      }
    },
    skillRating: {
      get: function () {
        return this.skill2.rating
      },
      set: function (newValue) {
        this.skill2.rating = newValue
      }
    },
  },
  methods: {
    closeSkillModal() {
      this.skillItem = this.skill.item
      this.skillRating = this.skill.rating
      this.$emit("handleEditSkillClosed")
    },
    saveSkillModal() {
      this.$emit("handleEditSkillSaved", { item: this.skill2.item, rating: this.skill2.rating})
    }
  }
}
</script>