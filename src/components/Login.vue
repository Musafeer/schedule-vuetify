<template>
  <v-layout align-center justify-center>
    <v-flex xs12 sm8 md4>
      <v-card class="elevation-12">
        <v-toolbar dark color="teal">
          <v-toolbar-title>Booking Form</v-toolbar-title>
        </v-toolbar>
        <v-card-text>
          <v-form>
            <v-text-field
              ref="name"
              v-model="name"
              :rules="[() => !!name || 'This field is required']"
              :error-messages="errorMessages"
              label="Full Name"
              placeholder="John Doe"
              required
            >
            </v-text-field>
            <v-text-field
              ref="address"
              v-model="email"
              :rules="[() => !!email || 'This field is required']"
              label="Email Address"
              placeholder="email@example.com"
              required
            >
            </v-text-field>
            <v-text-field
              ref="phone"
              v-model="phone"
              :rules="[() => !!phone || 'This field is required']"
              label="Phone Number"
              placeholder="012 345 6789"
              required
            >
            </v-text-field>
            <template>
              <v-menu
        v-model="menu2"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="date"
            label="Booking Date"
            prepend-icon="mdi-calendar"
            readonly
            v-bind="attrs"
            v-on="on"
            :rules="[() => !!date || 'This field is required']"
          ></v-text-field>
        </template>
        <v-date-picker
          v-model="date"
          @input="menu2 = false"
        ></v-date-picker>
      </v-menu>
            </template>
            <template>
              <v-dialog
                ref="dialog"
                v-model="modal2"
                :return-value.sync="time"
                persistent
                width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="time"
                    label="Time"
                    prepend-icon="mdi-clock-time-four-outline"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                    :rules="[() => !!time || 'This field is required']"
                  ></v-text-field>
                </template>
                <v-time-picker
                  v-if="modal2"
                  v-model="time"
                  :allowed-minutes="allowedStep"
                  format="24hr"
                  min="9:00"
                  max="16:30"
                  full-width
                >
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="modal2 = false">
                    Cancel
                  </v-btn>
                  <v-btn text color="primary" @click="$refs.dialog.save(time)">
                    OK
                  </v-btn>
                </v-time-picker>
              </v-dialog>
            </template>
            <v-autocomplete
              ref="service"
              v-model="service"
              :rules="[() => !!service || 'This field is required']"
              :items="service"
              label="Service"
              placeholder="Select..."
              required
            >
            </v-autocomplete>
          </v-form>
        </v-card-text>
        <v-divider class="mt-12"></v-divider>
        <v-card-actions>
          <v-btn text @click="cancel"> Cancel </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="submit"> Submit </v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  data: () => ({
    service: [
      "Identity Card",
      "Birth",
      "Death",
      "Adoption",
      "Marriage",
      "Divorce",
      "Citizenship",
    ],
    errorMessages: "",
    name: null,
    email: null,
    phone: null,
    date: null,
    menu2: false,
    time: null,
    modal2: false,
    services: null,
    formHasErrors: false,
  }),

  computed: {
    schedules: [],
    form() {
      return {
        name: this.name,
        email: this.email,
        phone: this.phone,
        date: this.date,
        time: this.time,
        services: this.services,
      };
    },
  },

  methods: {
    allowedStep: (m) => m % 15 === 0,
    submit() {
        this.$http.post("/schedules", this.form).then(() => {
                this.form = {
                    name: null,
                    email: null,
                    phone: null,
                    date: null,
                    time: null,
                    services: null,
                };
            });
    },
    cancel() {
        this.form = {
            name: null,
            email: null,
            phone: null,
            date: null,
            time: null,
            services: null,
        };
    },
  },
};
</script>