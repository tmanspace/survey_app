<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input
            type="radio"
            id="rating-poor"
            value="poor"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-great"
            value="great"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-great">Great</label>
        </div>
        <p v-if="invalidInput">
          One or more input fields are invalid. Please check your provided data.
        </p>
        <p v-if="serverRes">{{ serverRes }}</p>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<script>
export default {
  data() {
    return {
      enteredName: "",
      chosenRating: null,
      invalidInput: false,
      serverRes: "",
    };
  },
  // emits: ["survey-submit"],
  methods: {
    submitSurvey() {
      if (this.enteredName === "" || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;

      fetch(
        "https://vue-course-survey-app-91797-default-rtdb.europe-west1.firebasedatabase.app/surveys.json",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            name: this.enteredName,
            rating: this.chosenRating,
          }),
        }
      )
        .then((response) => {
          if (response.ok) {
            this.serverRes = "The survey result added successfully!";
          } else {
            this.serverRes = "There was a problem. Please, try again later.";
          }
        })
        .catch((err) => {
          this.serverRes = err.message;
        });

      // this.$emit("survey-submit", {
      //   userName: this.enteredName,
      //   rating: this.chosenRating,
      // });

      this.enteredName = "";
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type="text"] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>
