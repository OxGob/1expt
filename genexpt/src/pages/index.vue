<template>
  <q-page>
      <q-tabs color="secondary" glossy align="justify">
        <q-tab slot="title" default name="generalities" icon="subject" label="Generalities" />
        <q-tab slot="title" name="test" icon="verified_user" label="Test"/>
        <!-- General Tab -->
        <q-tab-pane name="generalities">Gen tab
          <!-- General Information Tab Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Generalities
              <span slot="subtitle">General information about the study</span>
            </q-card-title>
            <q-card-main>
              <q-field label="Title" helper="Short name of the study">
                <q-input v-model="studyTitle" clearable />
              </q-field>
              <q-field label="Description" helper="Layman's description of the study">
                <q-input v-model="studyDescription" type="textarea" rows="7" clearable />
              </q-field>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Principal Investigator Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
             <q-card-title>Principal Investigator(s)
              <span slot="subtitle">Information concerning the principal investigators</span>
            </q-card-title>
            <q-card-main>
              <q-field label="Name & Title" helper="Principal Investigator Name/Title">
                <q-input v-model="principalInvestigatorNameTitle" clearable />
              </q-field>
              <q-field label="Contact" helper="Contact Details, may include address">
                <q-input v-model="principalInvestigatorContactDetails" clearable />
              </q-field>
              <q-field label="Institution" helper="Name of Institution involved in study">
                <q-input v-model="principalInvestigatorInstitution" clearable />
              </q-field>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Institutions Card To implement lists with button -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-main>
            </q-card-main>
          </q-card>
          <!-- Gen Info Tab: Date Card -->
          <q-card class="bg-cyan-2 q-ma-xl">
            <q-card-title>Dates Of Study
              <span slot="subtitle">The Start and End Dates of the study</span>
            </q-card-title>
            <q-card-main>
              <div>
                <div>
                  <div class="row gutter-lg">
                    <div class="col-xs-4 col-md-4">
                        <q-field label="Start Date" />
                        </div>
                        <div class="col-xs-4 col-md-4">
                          <q-datetime class="q-ml-xl q-mb-lg" v-model="dateStart" type="date" format="D-MMM-YYYY" clearable />
                        </div>
                    </div>
                  </div>
              </div>
              <div>
                <div>
                  <div class="row gutter-lg">
                    <div class="col-xs-4 col-md-4">
                        <q-field label="End Date" />
                        </div>
                        <div class="col-xs-4 col-md-4">
                          <q-datetime class="q-ml-xl" v-model="dateEnd" type="date" format="D-MMM-YYYY" clearable />
                        </div>
                    </div>
                  </div>
              </div>
            </q-card-main>
          </q-card>
          <q-btn color="negative" class="float-right"  size="lg" label="SUBMIT" @click="convertToJSON">
          </q-btn>
          <!-- Gen Info Tab: Submit Test Button -->
        </q-tab-pane>
                <!-- TESTING Tab -->
        <q-tab-pane name="test">Test tab
          <q-card class="q-ma-xl">
            <q-card-title>Principal Investigator(s)
              <span slot="subtitle">Information concerning the principal investigators
                <br>
                <br>
              <q-btn class="q-ml-md" round size="sm" color="primary" icon="add" @click.native="addRowInvestigator" />
              </span>
            </q-card-title>
            <q-card-main>
              <div>
                <div>
                  <div class="row gutter-lg" id="investigator-form">
                    <div class="col-xs-4 col-md-4 col-lg-6">
                      <q-list highlight v-for="(input, index) in inputs" :key="input.id">
                        <q-item>
                          <q-btn round size="sm" color="negative" icon="remove" @click.native="removeRowInvestigator(index)" />
                        </q-item>
                          <q-item class="q-pr-sm">
                            <q-item-main label="Name & Title:" helper="Principal Investigator Name/Title" />
                            <q-input type="text" v-model="input.one" placeholder="NAME/TITLE" clearable/> {{ input.one }}
                          </q-item>
                          <q-item>
                            <q-item-main label="Contact:" />
                            <q-input type="textarea" v-model="input.two" rows=3 id="contact-investigator-form" placeholder="CONTACT" clearable/> {{ input.two }}
                          </q-item>
                          <q-item>
                            <q-item-main label="Institution:" />
                            <q-input type="textarea" placeholder="INSTITUTION" v-model="input.three" clearable /> {{ input.three }}
                          </q-item>
                      </q-list>
                    </div>
                  </div>
                </div>
              </div>
              <div id="repeat-after-Investigator"></div>
            </q-card-main>
          </q-card>
          <q-card class="bg-cyan-2 q-ma-xl">
              <q-card-title>Institution(s)
              <span slot="subtitle">Institutions that the data might or might not be shared with
                <br>
                <br>
              <q-btn class="q-ml-md" round size="sm" color="primary" icon="add" @click.native="addRowInvestigator" />
              </span>
            </q-card-title>
            <q-card-main>
              <div>
                <div>
                  <div class="row gutter-lg" id="institution-form">
                    <div class="col-xs-4 col-md-4 col-lg-6">
                      <q-list highlight v-for="(input, index) in inputs" :key="input.id">
                        <q-item>
                          <q-btn round size="sm" color="negative" icon="remove" @click.native="removeRowInvestigator(index)" />
                        </q-item>
                          <q-item class="q-pr-sm">
                            <q-item-main label="Name of Institution:" helper="Principal Investigator Name/Title" />
                            <q-input type="text" v-model="input.one" placeholder="NAME/TITLE" clearable/> {{ input.one }}
                          </q-item>
                          <q-item>
                            <q-item-main label="Contact Details of Institution:" />
                            <q-input type="textarea" v-model="input.two" rows=3 id="contact-investigator-form" placeholder="CONTACT" clearable/> {{ input.two }}
                          </q-item>
                          <q-item>
                            <q-item-main label="Data Access:" />
                            <q-radio v-model="radio1" val="one" color="secondary" label="NO" />
                            <q-radio v-model="radio1" val="two" color="amber" label="ANON" />
                            <q-radio v-model="radio1" val="three" color="red" label="FULL" />
                          </q-item>
                      </q-list>
                    </div>
                  </div>
                </div>
              </div>
              <div id="repeat-after-Investigator"></div>
            </q-card-main>
          </q-card>
        </q-tab-pane>
      </q-tabs>
  </q-page>
</template>

<script>

// var globalGridRowId = 0

export default {
  data () {
    return {
      studyTitle: '',
      studyDescription: '',
      principalInvestigatorNameTitle: '',
      principalInvestigatorContactDetails: '',
      principalInvestigatorInstitution: '',
      dateStart: null,
      dateEnd: null
    }
  },
  methods: {
    addRowInvestigator () {
      // increment the id
      this.inputs.push({
        one: '',
        two: '',
        three: ''
      })
    },
    removeRowInvestigator (index) {
      this.inputs.splice(index, 1)
    },
    convertToJSON () {
      this.$q.notify('Convert to JSON')
    }
  }
}
</script>

<style>
</style>
