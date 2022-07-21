<template>
  <form @submit.prevent="handleSubmit">
    <label>Email:</label>
    <input type="email" required v-model="email" />
    <label>Password:</label>
    <input type="password" required v-model="password" />
    <div v-for="error in passwordError" class="error">{{ error }}</div>

    <label>Role:</label>

    <select v-model="role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <label>Skills:</label>
    <input type="text" v-model="tempSkill" @keyup="addSkill" />
    <div v-for="skill in skills" :key="skill" class="pill">
      <span @click="deleteSkill(skill)">{{ skill }}</span>
    </div>

    <div class="terms">
      <input type="checkbox" required v-model="terms" />
      <label>I agree to the terms and conditions</label>
    </div>

    <div class="submit">
      <button type="submit">Create an Account</button>
    </div>

    <p>Email: {{ email }}</p>
    <p>Password: {{ password }}</p>
    <p>Role: {{ role }}</p>
    <p>Terms: {{ terms }}</p>
  </form>
</template>

<script>
export default {
  data() {
    return {
      email: "le@test.com",
      password: "",
      role: "select one",
      terms: false,
      skills: ["photoshop", "html", "css"],
      passwordError: [],
    };
  },
  methods: {
    addSkill(e) {
      if (e.key === ',' && this.tempSkill.length > 1) {
        this.tempSkill = this.tempSkill.substring(0, this.tempSkill.length - 1);

        if (!this.skills.includes(this.tempSkill)) {
          this.skills.push(this.tempSkill);
          console.log(this.tempSkill);
        }
        this.tempSkill = ''
      }
    },

    deleteSkill(e) {
      this.skills.splice(this.skills.indexOf(e), 1);
    },

    handleSubmit() {
      // password validation
      this.password.length >= 8 ? "" : this.passwordError.push("Password must be at least 8 characters");
      this.password.match(/[a-z]/) ? "" : this.passwordError.push("Password must contain at least one lowercase letter");
      this.password.match(/[A-Z]/) ? "" : this.passwordError.push("Password must contain at least one uppercase letter");
      this.password.match(/[0-9]/) ? "" : this.passwordError.push("Password must contain at least one number");
      this.password.match(/[^a-zA-Z0-9]/) ? "" : this.passwordError.push("Password must contain at least one special character");

      if (this.passwordError === "") {
        // submit form
        console.log("Form submitted");
      }
    }

  }
};
</script>

<style>
form {
  max-width: 420px;
  margin: 30px auto;
  background: White;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

input[type="checkbox"] {
  display: inline-block;
  width: 16px;
  margin: 0 10px 0 0;
  position: relative;
  top: 2px;
}

input,
select {
  display: block;
  width: 100%;
  padding: 10px 6px;
  border: none;
  box-sizing: border-box;
  border-bottom: 1px solid #ddd;
  color: #555;
}

.pill {
  display: inline-block;
  padding: 6px 12px;
  margin: 20px 20px 0 0;
  background: #eee;
  border-radius: 20px;
  font-size: 12px;
  color: #777;
  cursor: pointer;
}

button {
  background: DarkSlateGray;
  border: none;
  padding: 10px 20px;
  margin-top: 20px;
  color: White;
  border-radius: 20px;
}

.submit {
  text-align: center;
}

.error {
  color: Red;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}
</style>
