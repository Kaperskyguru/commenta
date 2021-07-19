<template>
  <div class="container">
    <ul class="comment-list">
      <Comment
        :key="comment.id"
        v-for="comment in comments"
        :comment="comment"
      ></Comment>
    </ul>
  </div>
</template>
    
    <script>
import { mapGetters } from "vuex";
import Comment from "./Comment";

export default {
  name: "Comments",
  components: { Comment },
  mounted() {
    this.$store.dispatch("GET_COMMENTS");

    this.listen();

    // //use your own credentials you get from Pusher
    // let pusher = new Pusher(1225973, {
    //   cluster: "eu",
    //   encrypted: false,
    // });

    // //Subscribe to the channel we specified in our Adonis Application
    // let channel = pusher.subscribe("comment-channel");

    // console.log(channel);
    // channel.bind("newComment", (data) => {
    //   this.$store.commit("ADD_COMMENT", data.comment);
    // });
  },
  methods: {
    listen() {
      Echo.channel("comment").listen("comment", (e) => {
        console.log(e);
        this.$store.commit("ADD_COMMENT", e);
      });
    },
  },
  computed: {
    ...mapGetters(["comments"]),
  },
};
</script>
    
    <style scoped>
.comment-list {
  padding: 1em 0;
  margin-bottom: 15px;
}
</style>