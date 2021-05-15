<template>
  <div>
    <v-row justify="center" class="mb-6">
      <v-card tile width="1000">
        <v-card-actions class="px-0">
          <v-btn
            v-show="!isPreview"
            icon
            x-large
            :ripple="false"
            @click="settingDialog = true">
            <v-icon :color="color">mdi-square-edit-outline</v-icon>
          </v-btn>
          <v-btn
            v-for="colorName in colorList" :key="colorName"
            fab
            x-small
            depressed
            :ripple="false"
            :color="colorName"
            @click="changeColor(colorName)">
            <v-icon v-if="color == colorName" color="white">mdi-check</v-icon>
          </v-btn>
          <v-spacer />
          <v-btn
            v-show="!isPreview"
            icon
            x-large
            :ripple="false"
            @click="captureImage">
            <v-icon :color="color">mdi-download</v-icon>
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-row>
    <v-row justify="center">
      <v-card tile id="capture" :color="color" elevation="8" min-width="1000">
        <v-card-text>
          <v-row>
            <v-col cols="5" style="background-color: #EEEEEE; min-height: 100%;">
              <p class="display-1 text-center font-weight-bold pa-3" style="border: 2px solid; border-radius: 50px;">{{ name }}</p>
              <p class="title font-weight-bold mb-2">HOBBY</p>
              <div class="pb-6" style="border-bottom: 2px solid; white-space: pre-line;">{{ hobby }}</div>
              <p class="title font-weight-bold mb-2 pt-4">SKILLS</p>
              <v-card-actions v-for="skill in skills" :key="skill.item">
                <span>{{ skill.item }}</span>
                <v-spacer />
                <v-rating
                  :value="skill.rating"
                  background-color="grey"
                  :color="color"
                  readonly
                  dense />
              </v-card-actions>
            </v-col>
            <v-col cols="7" style="min-height: 100%;">
              <p class="title font-weight-bold white--text mb-2">PROFILE</p>
              <div class="pb-6 white--text" style="border-bottom: 2px solid; white-space: pre-line;">{{ profile }}</div>
              <p class="title font-weight-bold white--text mb-2 pt-4">STRENGTH</p>
              <div class="pb-6 white--text" style="border-bottom: 2px solid;">
                <v-chip v-for="(strength, i) in strength" :key="i" class="mx-1" color="grey lighten-3">
                  {{ i + 1 }}. {{ strength }}
                </v-chip>
              </div>
              <p class="title font-weight-bold white--text mb-2 pt-4">EXPERIENCE</p>
              <v-timeline dense>
                <v-timeline-item
                  v-for="(experience, i) in experience"
                  :key="i"
                  :color="color"
                  small
                  right>
                  <div>
                    <div class="subtitle-2 white--text">{{ experience.period }}</div>
                    <h2 class="subtitle-1 font-weight-bold white--text">{{ experience.project }}</h2>
                    <div class="subtitle-2 white--text" style="white-space: pre-line;">{{ experience.content }}</div>
                  </div>
                </v-timeline-item>
              </v-timeline>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-row>
    <v-dialog
      v-model="settingDialog"
      width="600">
      <v-card>
        <v-card-text>
          <v-text-field
            v-model="name"
            label="NAME" />
          <v-text-field
            v-model="hobby"
            label="HOBBY" />
          <v-text-field
            v-model="skills"
            label="SKILLS" />
          <v-textarea
            v-model="profile"
            label="PROFILE" />
          <v-combobox
            v-model="strength"
            :items="strength"
            hide-selected
            label="STRENGTH"
            multiple
            chips />
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  data () {
    return {
      fab: false,
      settingDialog: true,
      color: "green lighten-2",
      colorList: [
        "green lighten-2",
        "blue lighten-2",
        "red lighten-2",
        "purple lighten-2",
        "yellow darken-2",
        "brown lighten-2",
        "grey darken-4"
      ],
      name: "",
      hobby: "",
      skills: [
      ],
      profile: "",
      strength: [
      ],
      experience: [
      ],
    }
  },
  methods: {
    captureImage () {
      this.$html2canvas(document.querySelector('#capture')).then((canvas) => {
        const link = document.createElement('a')
        link.href = canvas.toDataURL()
        link.download = `resume.png`
        link.click()
      })
    },
    changeColor(color) {
      this.color = color;
    }
  }
}
</script>