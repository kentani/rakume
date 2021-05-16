<template>
  <div>
    <v-row justify="center" class="mb-6">
      <v-card tile width="1000">
        <v-card-actions>
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
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card flat class="mb-6" style="background-color: #EEEEEE; border: 2px solid; border-radius: 50px;">
                    <v-card-text>
                      <h2 class="display-1 grey--text text--darken-2 text-center font-weight-bold mb-0">{{ name }}</h2>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute>
                        <v-btn
                          rounded
                          @click="openNameModal">
                          編集する
                        </v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-dialog
                v-model="nameModal"
                width="600">
                <v-card>
                  <v-card-title>
                    <span class="headline">Edit Name</span>
                  </v-card-title>
                  <v-card-text>
                    <v-text-field
                      v-model="name"
                      filled
                      shaped
                      hide-details
                      :color="color" />
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="closeNameModal(false)">close</v-btn>
                    <v-btn text @click="closeNameModal">save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <h3 class="title grey--text text--darken-2 font-weight-bold mb-0">HOBBY</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat style="background-color: #EEEEEE; white-space: pre-line;">
                    <v-card-text class="body-2 grey--text text--darken-1 pa-2">
                      <p class="mb-0">{{ hobby }}</p>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded @click="openHobbyModal">編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-dialog
                v-model="hobbyModal"
                width="600">
                <v-card>
                  <v-card-title>
                    <span class="headline">Edit Hobby</span>
                  </v-card-title>
                  <v-card-text>
                    <v-textarea
                      v-model="hobby"
                      filled
                      shaped
                      hide-details
                      :color="color" />
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="closeHobbyModal(false)">close</v-btn>
                    <v-btn text @click="closeHobbyModal">save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-divider class="mt-3 mb-6" style="border: 1px solid black; border-color: black;" />
              <h3 class="title grey--text text--darken-2 font-weight-bold mb-0">SKILLS</h3>
              <div
                v-for="(skill, i) in skills"
                :key="skill.item">
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat style="background-color: #EEEEEE;">
                    <v-card-actions
                      class="body-2 grey--text text--darken-1">
                      <span>{{ skill.item }}</span>
                      <v-spacer />
                      <v-rating
                        :value="skill.rating"
                        background-color="grey"
                        :color="color"
                        readonly
                        dense />
                    </v-card-actions>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded @click="openSkillsModal(i)">編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              </div>
              <v-dialog
                v-model="skillsModal"
                width="600">
                <v-card>
                  <v-card-title>
                    <span class="headline">Edit Skills</span>
                  </v-card-title>
                  <v-card-text>
                    <v-card-actions
                      class="body-2 grey--text text--darken-1">
                      <v-text-field
                        v-model="skillItem"
                        :color="color"
                        hide-details />
                      <v-spacer />
                      <v-rating
                        :value="0"
                        background-color="grey"
                        :color="color"
                        dense />
                    </v-card-actions>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="closeSkillsModal(false)">close</v-btn>
                    <v-btn text @click="closeSkillsModal">save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-col>
            <v-col cols="7" style="min-height: 100%;">
              <h3 class="title font-weight-bold white--text mb-0">PROFILE</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat :color="color" style="white-space: pre-line;">
                    <v-card-text class="body-2 white--text pa-2">
                      <p class="mb-0">{{ profile }}</p>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded @click="openProfileModal">編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-dialog
                v-model="profileModal"
                width="600">
                <v-card>
                  <v-card-title>
                    <span class="headline">Edit Profile</span>
                  </v-card-title>
                  <v-card-text>
                    <v-textarea
                      v-model="profile"
                      filled
                      shaped
                      hide-details
                      :color="color" />
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="closeProfileModal(false)">close</v-btn>
                    <v-btn text @click="closeProfileModal">save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-divider class="mt-3 mb-6" style="border: 1px solid white; border-color: white;" />
              <h3 class="title font-weight-bold white--text mb-0">STRENGTH</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat :color="color" style="white-space: pre-line;">
                    <v-card-text class="body-2 white--text pa-2">
                      <v-chip v-for="(strength, i) in strength" :key="i" class="mx-1" color="grey lighten-3">
                        <span class="body-2 grey--text text--darken-2">{{ i + 1 }}. {{ strength }}</span>
                      </v-chip>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded @click="openStrengthModal">編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-dialog
                v-model="strengthModal"
                width="600">
                <v-card>
                  <v-card-title>
                    <span class="headline">Edit Strength</span>
                  </v-card-title>
                  <v-card-text>
                    <v-combobox
                      v-model="strength"
                      :items="strength"
                      :color="color"
                      :item-color="color"
                      multiple
                      clearable
                      hide-details
                      chips />
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="closeStrengthModal(false)">close</v-btn>
                    <v-btn text @click="closeStrengthModal">save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-divider class="mt-3 mb-6" style="border: 1px solid white; border-color: white;" />
              <h3 class="title font-weight-bold white--text mb-0">EXPERIENCE</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat :color="color" style="white-space: pre-line;">
                    <v-card-text class="body-2 white--text pa-2">
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
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded @click="openExperienceModal">編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-dialog
                v-model="experienceModal"
                width="600">
                <v-card>
                  <v-card-title>
                    <span class="headline">Edit Experience</span>
                  </v-card-title>
                  <v-card-text>
                    <v-textarea
                      v-model="experience"
                      filled
                      shaped
                      hide-details
                      :color="color" />
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer />
                    <v-btn text @click="closeExperienceModal(false)">close</v-btn>
                    <v-btn text @click="closeExperienceModal">save</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-row>
  </div>
</template>
<script>
export default {
  data () {
    return {
      fab: false,
      settingDialog: false,
      nameModal: false,
      hobbyModal: false,
      skillsModal: false,
      profileModal: false,
      strengthModal: false,
      experienceModal: false,
      color: "green lighten-2",
      colorList: [
        "green lighten-2",
        "blue lighten-2",
        "red lighten-2",
        "purple lighten-2",
        "yellow darken-2",
        "brown lighten-2",
        "grey darken-2",
        "grey darken-4"
      ],
      name: "山田 太郎",
      hobby: "#プログラミング",
      skills: [
        { item: "Ruby", rating: 3 },
      ],
      profile: "・要件定義からリリース、運用保守まで様々な工程での開発経験があります",
      strength: [
        "達成欲",
      ],
      experience: [
        { period: "2021.01 - 現在", project: "○○システム開発", content: "機能開発、運用保守を行いました。" },
      ],
      skillIndex: "",
      skillItem: "",
      tmpName: "",
      tmpHobby: "",
      tmpSkillItem: "",
      tmpProfile: "",
      tmpStrength: "",
      tmpExperience: "",
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
    },
    openNameModal() {
      this.tmpName = this.name
      this.nameModal = true
    },
    closeNameModal(isSave = true) {
      if (!isSave) this.name = this.tmpName;
      this.nameModal = false
    },
    openHobbyModal() {
      this.tmpHobby = this.hobby
      this.hobbyModal = true
    },
    closeHobbyModal(isSave = true) {
      if (!isSave) this.hobby = this.tmpHobby;
      this.hobbyModal = false
    },
    openSkillsModal(i) {
      this.skillIndex = i
      this.skillItem = this.skills[i].item
      this.tmpSkillItem = this.skills[i].item
      this.skillsModal = true
    },
    closeSkillsModal(isSave = true) {
      if (isSave) {
        this.skills[this.skillIndex].item = this.skillItem
        this.skillItem = this.tmpSkillItem;
      } else {
        this.skillItem = this.tmpSkillItem;
      }
      this.skillsModal = false
    },
    openProfileModal() {
      this.tmpProfile = this.profile
      this.profileModal = true
    },
    closeProfileModal(isSave = true) {
      if (!isSave) this.profile = this.tmpProfile;
      this.profileModal = false
    },
    openStrengthModal() {
      this.tmpStrength = this.strength
      this.strengthModal = true
    },
    closeStrengthModal(isSave = true) {
      if (!isSave) this.strength = this.tmpStrength;
      this.strengthModal = false
    },
    openExperienceModal() {
      this.tmpExperience = this.experience
      this.experienceModal = true
    },
    closeExperienceModal(isSave = true) {
      if (!isSave) this.experience = this.tmpExperience;
      this.experienceModal = false
    },
    handleEditSkillItem(newItem, i) {
      this.tmpSkillItem = newItem
      this.skills[i].item = newItem
    }
  }
}
</script>