<template>
  <q-page class="flex flex-center">
    <div class="bg-grey-8" style="max-width: 700px;min-width: 700px">
      <q-toolbar class="bg-primary text-white shadow-2 flex-center">
        <q-img
          src="../assets/Images/ApplicationIcon2.png"
          spinner-color="white"
          style="height: 60px; max-width: 200px"
          img-class="my-custom-image"
          class="rounded-borders"
        >
        </q-img>
      </q-toolbar>

      <q-list class="">
        <q-item
          v-for="contact in contacts"
          :key="contact.id"
          class="q-my-sm"
          clickable
          v-ripple
        >
          <q-item-section avatar>
            <q-avatar color="deep-purple-1" text-color="primary">
              {{ contact.letter }}
            </q-avatar>
          </q-item-section>

          <q-item-section>
            <q-item-label>{{ contact.name }}</q-item-label>
            <q-item-label caption lines="1">{{ contact.number }}</q-item-label>
          </q-item-section>

          <q-item-section side>
            <div class="row">
              <div class="col">
                <q-btn
                  flat
                  round
                  color="deep-purple-1"
                  icon="edit"
                  @click="edit(contact)"
                />
              </div>
              <div class="col">
                <q-btn
                  flat
                  round
                  color="deep-purple-1"
                  icon="delete"
                  @click="remove(contact)"
                />
              </div>
            </div>
          </q-item-section>
        </q-item>
      </q-list>
      <q-btn flat color="white" class="full-width" to="new" icon="add" />
    </div>

    <q-dialog v-model="EditContack_dialog" persistent class="bg-grey-8">
      <q-card style="min-width: 350px " class="bg-grey-8">
        <q-card-section class="bg-primary text-white">
          <div class="text-h6">Edit Your Contack</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
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
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn
            flat
            color="white"
            class="full-width "
            icon="check"
            to="index"
            @click="SaveDetails()"
            v-close-popup
          />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      contacts: [],
      EditContack_dialog: false,
      Contack: {
        name: "",
        number: ""
      }
    };
  },

  created() {
    /* Run this code to remove from storage
      window.localStorage.removeItem("Contacks");
    */

    var myStorage = window.localStorage;
    this.getContacks();
  },

  methods: {
    getContacks() {
      var Data = window.localStorage.getItem("Contacks");
      var holder = JSON.parse(Data);
      this.contacts = holder;
    },

    edit(contackSelected) {
      this.EditContack_dialog = true;
      this.Contack = this.contacts.find(x => x.ID == contackSelected.ID);
    },

    remove(contackSelected) {
      var holder = this.contacts.filter(x => x.ID != contackSelected.ID);
      this.contacts = holder;

      window.localStorage.setItem("Contacks", JSON.stringify(holder));
    },

    SaveDetails() {
      this.remove(this.Contack);
      var Data = window.localStorage.getItem("Contacks");
      var holder = JSON.parse(Data);

      holder.push(this.Contack);
      window.localStorage.setItem("Contacks", JSON.stringify(holder));

      this.$q.notify({
        icon: "check",
        color: "positive",
        message: "Changes Saved",
        position: "top"
      });

      this.getContacks();
    }
  }
};
</script>
