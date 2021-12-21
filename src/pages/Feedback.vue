<template>
  <q-page class="text-white" style="overflow: hidden">
    <div class="q-pa-md flex row">
      <img
        class="col"
        style="
          width: 100px;
          height: 100px;
          border-radius: 50%;
          margin-right: 20px;
        "
        width="150"
        src="https://avataaars.io/?avatarStyle=Transparent&topType=LongHairStraight&accessoriesType=Blank&hairColor=BrownDark&facialHairType=Blank&clotheType=BlazerShirt&eyeType=Default&eyebrowType=Default&mouthType=Default&skinColor=Light"
        alt=""
      />
      <div class="col text-h4">Rate this speaker</div>
    </div>
    <q-card>
      <q-card-section class="bg-dark">
        <q-expansion-item
          class="text-white"
          label="Your Info (optional)"
          :expanded="true"
        >
          <q-input
            class="q-ma-md"
            dense
            outlined
            v-model="name"
            bg-color="white"
            label="Name"
          />
          <q-input
            class="q-ma-md"
            dense
            outlined
            v-model="roll"
            bg-color="white"
            label="Roll No."
          />
          <q-input
            class="q-ma-md"
            dense
            outlined
            v-model="email"
            bg-color="white"
            label="Email"
          />
        </q-expansion-item>
      </q-card-section>
      <q-card-section class="bg-dark">
        <div class="row">
          <div class="text-h5">Did you understand the basic idea?</div>
          <q-rating
            v-model="ratingBasic"
            class="col-grow"
            min="1"
            max="5"
            size="3em"
            color="yellow"
            icon="star_border"
            icon-selected="star"
            icon-half="star_half"
            no-dimming
          />
          <span class="flex flex-center col text-h5">{{ ratingBasic }}</span>
        </div>
      </q-card-section>

      <q-card-section class="bg-dark">
        <div class="text-h5">Slides</div>
        <div class="row">
          <q-rating
            v-model="ratingSlides"
            class="col-grow"
            min="1"
            max="5"
            size="3em"
            color="yellow"
            icon="star_border"
            icon-selected="star"
            icon-half="star_half"
            no-dimming
          />
          <span class="flex flex-center col text-h5">{{ ratingSlides }}</span>
        </div>
      </q-card-section>
      <q-card-section class="bg-dark">
        <div class="text-h5">Fluency</div>
        <div class="row">
          <q-rating
            v-model="ratingFluency"
            class="col-grow"
            min="1"
            max="5"
            size="3em"
            color="yellow"
            icon="star_border"
            icon-selected="star"
            icon-half="star_half"
            no-dimming
          />
          <span class="flex flex-center col text-h5">{{ ratingFluency }}</span>
        </div>
      </q-card-section>
      <q-card-section class="bg-dark">
        <div class="text-h5">Overall</div>
        <div class="row">
          <q-rating
            v-model="ratingOverall"
            class="col-grow"
            min="1"
            max="5"
            size="3em"
            color="yellow"
            icon="star_border"
            icon-selected="star"
            icon-half="star_half"
            no-dimming
          />
          <span class="flex flex-center col text-h5">{{ ratingOverall }}</span>
        </div>
      </q-card-section>
      <q-card-section class="bg-dark">
        <div class="text-h5 q-mb-md">Message</div>
        <q-input v-model="message" dark filled type="textarea" />
      </q-card-section>
      <q-card-section class="bg-dark flex justify-end">
        <q-btn
          @click="submit"
          color="primary"
          label="Send👌"
          size="lg"
          class="q-ma-md"
        />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";
export default {
  name: "PageIndex",
  components: {},
  data() {
    return {
      name: "",
      roll: "",
      email: "",
      ratingBasic: 0,
      ratingSlides: 0,
      ratingFluency: 0,
      ratingOverall: 0,
      message: "",
    };
  },
  mounted() {
    this.$q
      .dialog({
        title: "Alert",
        message: "Some message",
      })
      .onOk(() => {
        // console.log('OK')
      })
      .onCancel(() => {
        // console.log('Cancel')
      })
      .onDismiss(() => {
        // console.log('I am triggered on both OK and Cancel')
      });
    this.$q.dialog({
      title: "Feedback",
      message:
        "Please rate the speaker and leave a message. Your feedback will help us improve the experience.",
      persistent: true,
    });
    //if no '8080' in url then check completed
    if (window.location.href.includes("8080")) this.checkCompleted();
  },
  methods: {
    checkCompleted() {
      if (
        JSON.parse(localStorage.getItem("completed_ids")).includes(
          this.$route.query.id
        )
      ) {
        this.$q.notify({
          color: "negative",
          message: "You have already rated this speaker",
          icon: "error",
        });
        this.$router.push("/");
      }
    },
    async submit() {
      const payload = {
        //get the profile id from the url params
        id: this.$route.query.id,
        name: this.name || "Anonymous",
        roll: this.roll || "Anonymous",
        email: this.email || "Anonymous",
        ratingBasic: this.ratingBasic,
        ratingSlides: this.ratingSlides,
        ratingFluency: this.ratingFluency,
        ratingOverall: this.ratingOverall,
        message: this.message,
      };
      try {
        const res = await axios.post(
          // "http://localhost:5000/sendFeedback",
          "https://feedbax.plasmatch.in/sendFeedback",
          payload
        );
        if (res.status === 200) {
          this.saveInfo(payload.id);
        }
        console.log(res);
        this.$q
          .dialog({
            title: "Thanks🙌",
            dark: true,
            message: `Your feedback has been submitted.
            This will help us improve the experience.`,
            persistent: true,
            position: "bottom",
          })
          .onOk(() => {
            this.$router.push("/");
            // console.log('OK')
          });
      } catch (err) {
        console.log(err);
      }
    },
    saveInfo(id) {
      if (localStorage.getItem("completed_ids") === null) {
        localStorage.setItem("completed_ids", JSON.stringify([id]));
      } else {
        localStorage.setItem(
          "completed_ids",
          JSON.stringify([
            ...JSON.parse(localStorage.getItem("completed_ids")),
            id,
          ])
        );
      }
      console.log(localStorage.getItem("completed_ids"));
    },
  },
};
</script>
