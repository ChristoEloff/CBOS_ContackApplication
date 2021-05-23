<template>
  <q-page class="flex flex-center">
    <div class="bg-grey-8" style="max-width: 700px;min-width: 700px">
      <q-toolbar class="bg-primary text-white shadow-2 flex-center">
        <q-btn flat round color="white" icon="arrow_back" to="/index" />
        <q-toolbar-title>Add New Contact</q-toolbar-title>
      </q-toolbar>

      <q-form class=" q-pa-md ">
        <q-input
          ref="name"
          v-model="Contack.name"
          label="Name"
          color="deep-purple-1"
          label-color="deep-purple-1"
          :rules="[val => !!val || '*Field is required']"
          lazy-rules
          dark
        />

        <q-input
          ref="number"
          v-model="Contack.number"
          label="Number"
          color="deep-purple-1"
          label-color="deep-purple-1"
          :rules="[val => !!val || '*Field is required']"
          lazy-rules
          dark
        />
      </q-form>
      <q-btn
        flat
        color="white"
        class="full-width "
        icon="check"
        @click="addUser"
      />
    </div>
  </q-page>
</template>

<script>
import { uuid } from "uuidv4";

export default {
  data() {
    return {
      Contack: {
        name: "",
        number: ""
      }
    };
  },

  methods: {
    addUser() {
      if (this.validateForm()) {
        var newContack = {
          ID: uuid(),
          name: this.Contack.name,
          number: this.Contack.number,
          letter: this.Contack.name.charAt(0).toUpperCase()
        };

        var myStorage = window.localStorage;
        var holder = [];
        if (!myStorage.Contacks) {
          holder = [newContack];
        } else {
          var Data = window.localStorage.getItem("Contacks");
          holder = JSON.parse(Data);

          holder.push(newContack);
        }

        window.localStorage.setItem("Contacks", JSON.stringify(holder));

        this.$q.notify({
          icon: "add",
          color: "positive",
          message: "Contack Saved",
          position: "top"
        });

        this.$router.push("/index");
      }
    },

    validateForm() {
      var response = false;
      this.$refs.name.validate();
      this.$refs.number.validate();

      if (this.$refs.name.hasError || this.$refs.number.hasError) {
        this.$q.notify({
          icon: "error",
          color: "negative",
          message: "Form Validation failed",
          position: "top",
          actions: [
            {
              label: "Dismiss",
              color: "yellow",
              handler: () => {
                this.$refs.name.resetValidation();
                this.$refs.number.resetValidation();
              }
            }
          ]
        });
      } else {
        response = true;
      }
      return response;
    }
  }
};
</script>
