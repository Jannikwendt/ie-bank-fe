<template>
  <div class="jumbotron vertical-center">
    <div class="container">
      <div class="row">
        <div class="col-sm-12">
          <h1>Accounts</h1>
          <hr />
          <br />

          <!-- Alert Message -->
          <b-alert v-if="showMessage" variant="success" show>
            {{ message }}
          </b-alert>

          <button
            type="button"
            class="btn btn-success btn-sm"
            v-b-modal.account-modal
          >
            Create Account
          </button>
          <br /><br />

          <table class="table table-hover">
            <thead>
              <tr>
                <th scope="col">Account Name</th>
                <th scope="col">Account Number</th>
                <th scope="col">Account Balance</th>
                <th scope="col">Account Currency</th>
                <!-- NEW header -->
                <th scope="col">Country</th>
                <th scope="col">Account Status</th>
                <th scope="col">Actions</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="account in accounts" :key="account.id">
                <td>{{ account.name }}</td>
                <td>{{ account.account_number }}</td>
                <td>{{ account.balance }}</td>
                <td>{{ account.currency }}</td>
                <!-- NEW cell -->
                <td>{{ account.country }}</td>
                <td>
                  <span
                    v-if="account.status === 'Active'"
                    class="badge badge-success"
                  >
                    {{ account.status }}
                  </span>
                  <span v-else class="badge badge-danger">
                    {{ account.status }}
                  </span>
                </td>
                <td>
                  <div class="btn-group" role="group">
                    <button
                      type="button"
                      class="btn btn-info btn-sm"
                      v-b-modal.edit-account-modal
                      @click="editAccount(account)"
                    >
                      Edit
                    </button>
                    <button
                      type="button"
                      class="btn btn-danger btn-sm"
                      @click="deleteAccount(account)"
                    >
                      Delete
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>

          <footer class="text-center">
            Copyright &copy; All Rights Reserved.
          </footer>
        </div>
      </div>

      <!-- Modal: Create Account -->
      <b-modal
        ref="addAccountModal"
        id="account-modal"
        title="Create a new account"
        hide-backdrop
        hide-footer
      >
        <b-form @submit="onSubmit" class="w-100">
          <!-- Name Field -->
          <b-form-group label="Account Name:" label-for="form-name-input">
            <b-form-input
              id="form-name-input"
              type="text"
              v-model="createAccountForm.name"
              placeholder="Account Name"
              required
            />
          </b-form-group>

          <!-- Currency Field -->
          <b-form-group label="Currency:" label-for="form-currency-input">
            <b-form-input
              id="form-currency-input"
              type="text"
              v-model="createAccountForm.currency"
              placeholder="$ or €"
              required
            />
          </b-form-group>

          <!-- Country Field -->
          <b-form-group label="Country:" label-for="form-country-input">
            <b-form-input
              id="form-country-input"
              type="text"
              v-model="createAccountForm.country"
              placeholder="e.g. ES"
              required
            />
          </b-form-group>

          <b-button type="submit" variant="outline-info">Submit</b-button>
        </b-form>
      </b-modal>

      <!-- Modal: Edit Account (unchanged) -->
      <b-modal
        ref="editAccountModal"
        id="edit-account-modal"
        title="Edit the account"
        hide-backdrop
        hide-footer
      >
        <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group label="Account Name:" label-for="form-edit-name-input">
            <b-form-input
              id="form-edit-name-input"
              type="text"
              v-model="editAccountForm.name"
              required
            />
          </b-form-group>
          <b-button type="submit" variant="outline-info">Update</b-button>
        </b-form>
      </b-modal>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AppAccounts",
  data() {
    return {
      accounts: [],
      createAccountForm: {
        name: "",
        currency: "",
        country: "",
      },
      editAccountForm: {
        id: "",
        name: "",
      },
      showMessage: false,
      message: "",
    };
  },
  methods: {
    /*************** REST ***************/
    RESTgetAccounts() {
      axios
        .get(`${process.env.VUE_APP_ROOT_URL}/accounts`)
        .then((res) => (this.accounts = res.data.accounts))
        .catch((err) => console.error(err));
    },
    RESTcreateAccount(payload) {
      axios
        .post(`${process.env.VUE_APP_ROOT_URL}/accounts`, payload)
        .then(() => {
          this.RESTgetAccounts();
          this.message = "Account created successfully!";
          this.showMessage = true;
          setTimeout(() => (this.showMessage = false), 3000);
        })
        .catch((err) => console.error(err));
    },

    /*************** FORM ***************/
    initForm() {
      this.createAccountForm = { name: "", currency: "", country: "" };
      this.editAccountForm = { id: "", name: "" };
    },
    onSubmit(e) {
      e.preventDefault();
      this.$refs.addAccountModal.hide();
      this.RESTcreateAccount({ ...this.createAccountForm });
      this.initForm();
    },

    // editAccount, deleteAccount, onSubmitUpdate unchanged…
  },
  created() {
    this.RESTgetAccounts();
  },
};
</script>
