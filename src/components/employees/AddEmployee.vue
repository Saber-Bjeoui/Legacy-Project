<template>
  <div class="container">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <br />
      <b-form-group id="input-group-0" label-for="input-0">
        <b-form-input
          id="input-0"
          v-model="employee.username"
          required
          placeholder="Enter user-name"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-1" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="employee.last_name"
          required
          placeholder="Enter last-name"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="employee.first_name"
          required
          placeholder="Enter first-name"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-1" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="employee.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>
      <label>Select birthday:</label>
      <b-form-datepicker
        id="example-datepicker"
        v-model="employee.birthday"
        class="mb-2"
      ></b-form-datepicker>

      <b-form-group
        id="input-group-3"
        label="Select Position:"
        label-for="input-3"
      >
        <b-form-select
          id="input-3"
          v-model="employee.job_position"
          :options="positions"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group
        id="input-group-3"
        label="Select gender:"
        label-for="input-3"
      >
        <b-form-select
          id="input-3"
          v-model="employee.gender"
          :options="genders"
          required
        ></b-form-select>
      </b-form-group>

      <b-button class="btn-width" type="submit" variant="success">{{
        this.$route.path === "/employees/add" ? "Submit" : "Update"
      }}</b-button>
      <b-button class="btn-width" type="reset" variant="primary"
        >Reset</b-button
      >
      <br />
      <br />
    </b-form>

    <!-- <pre class="m-0">{{ employee }}</pre> -->
  </div>
</template>

<script>
import axios from "axios";
// import { SMTPClient } from "emailjs";

// const client = new SMTPClient({
//   user: "user",
//   password: "password",
//   host: "smtp.your-email.com",
//   ssl: true,
// });

export default {
  data() {
    return {
      employee: {
        email: "",
        first_name: "",
        last_name: "",
        username: "",
        job_position: "",
        gender: "",
        birthday: "",
      },

      positions: [
        { text: "Select One", value: null },
        "Director",
        "Cleaner",
        "Developper",
        "Designer",
      ],
      genders: [{ text: "Select One", value: null }, "Male", "Female"],
      show: true,
    };
  },
  mounted: async function() {
    console.log(this.$route);
    if (this.$route.params.id) {
      const result = await axios.get(`/api/users/${this.$route.params.id}`);
      console.log("data ====> ", result.data);
      this.employee = result.data;
      console.log(this.employee);
    }
  },
  methods: {
    async onSubmit(evt) {
      evt.preventDefault();
      // alert(JSON.stringify(this.form));
      var url = "/api/users/add";
      if (this.$route.params.id) {
        url = "/api/users/update/" + this.$route.params.id;
      }
      try {
        console.log(url);
        const data = await axios.post(url, this.employee);
        // `${this.employee.email}`,
        // client.send(
        //   {
        //     text: `Hello ${this.employee.first_name} ${this.employee.last_name} this your default credientilals "Username: ${this.employee.username}, password: ${this.employee.password}`,
        //     from: "bjeoui.saber@gmail.com",
        //     to: "bjsaber@gmail.com",
        //     subject: "Your default credientials",
        //   },
        //   (err, message) => {
        //     console.log(err || message);
        //   }
        // );
        console.log(data.data);
      } catch (error) {
        console.error(error);
      }
      this.$router.push("/employees");
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.employee.email = "";
      this.employee.name = "";
      this.employee.food = null;
      this.employee.checked = [];
      this.employee.date = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
};
</script>
<style scoped>
.container {
  border: 1px solid black;
  border-radius: 10px;
  width: 50%;
}
.btn-width {
  width: 100%;
  margin: 10px;
}
</style>
