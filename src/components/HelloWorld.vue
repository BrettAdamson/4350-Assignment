<template>
  <div>
    <b-row>
      <b-col></b-col>
      <b-col class="mt-4 pt-4">
        <h1>COMP 4350 Assignment</h1>
        <b-form-input v-model="form.tag" type="text" placeholder="Enter tag" required></b-form-input>
        <b-button type="submit" variant="primary" class="mt-4" @click="submit()">Submit</b-button>
      </b-col>
      <b-col></b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        tag: ""
      },
      combinedQuestionsArray: [],
      combinedAnswersArray: [],
      answerIDsArray: [],
      answerIDsString: ""
    };
  },
  methods: {
    async submit(){
    var questionsArray  = await this.queryResults()
    console.log(questionsArray)
    
    var questionIDString = await this.getQuestionIDString(questionsArray)
    console.log(questionIDString)

    },
    async queryResults() {
      var now = new Date();
      var oneWeekAgo = new Date();
      oneWeekAgo.setDate(now.getDate() - 7);
      now = Math.floor(now.getTime() / 1000);
      oneWeekAgo = Math.floor(oneWeekAgo.getTime() / 1000);
      var tag = encodeURIComponent(this.form.tag);
      var ratedArray = []
      var recentArray = []
     await this.$axios
        .get(
             "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
        oneWeekAgo +
        "&todate=" +
        now +
        "&order=desc&sort=activity&tagged=" +
        tag +
        "&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
         recentArray = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });

      await this.$axios
        .get(
        "https://api.stackexchange.com/2.2/questions?page=1&pagesize=10&fromdate=" +
        oneWeekAgo +
        "&todate=" +
        now +
        "&order=desc&sort=votes&tagged=" +
        tag +
        "&site=stackoverflow&filter=withbody"
        )
        .then(function(response) {
          // handle success
           ratedArray = response.data.items;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });

          this.combinedQuestionsArray =  ratedArray.concat(recentArray);
      //Sort the questions
      //use Array.prototype.sort https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
      this.combinedQuestionsArray.sort(function(a, b) {
        return a.creation_date - b.creation_date;
      });
          return this.combinedQuestionsArray
    },
    async getQuestionIDString(questionArray) {
      //Get the ID's of each question
      var questionIDsString = ""
      for (var x = 0; x < questionArray.length; x++) {
        questionIDsString += ";"+ questionArray[x].question_id;
      }
      questionIDsString = questionIDsString.substring(1); //remove first semicolon
      return questionIDsString
    },

    async getAnswers() {
      // this.$axios
      //   .get(
      //     "/2.2/questions/" +
      //       this.questionIDsString +
      //       "/answers?page=1&pagesize=100&order=desc&sort=activity&site=stackoverflow&filter=withbody"
      //   )
      //   .then(function(response) {
      //     // handle success
      //     console.log(response);
      //     //  this.combinedAnswersArray = ratedResponse.data.items
      //   })
      //   .catch(function(error) {
      //     // handle error
      //     console.log(error);
      //   });

      //Stringify the answer ID's with semicolon delimiter
      for (var x = 0; x < this.combinedAnswersArray.length; x++) {
        this.answerIDsArray[x] = this.combinedAnswersArray[x].questionID;
      }
    },
    getCommentsForQuestion() {
      // this.$axios
      //   .get(
      //     "https://api.stackexchange.com/2.2/posts/"+this.questionIDsString+"/comments?order=desc&sort=creation&site=stackoverflow&filter=withbody"
      //   )
      //   .then(function(response) {
      //     // handle success
      //      this.combinedAnswersArray = ratedResponse.data.items
      //   })
      //   .catch(function(error) {
      //     // handle error
      //     console.log(error);
      //   });
    },
    getCommentsForAnswers() {
      // this.$axios
      //   .get(
      //     ""https://api.stackexchange.com/2.2/posts/"+this.answerIDsArray+"/comments?order=desc&sort=creation&site=stackoverflow&filter=withbody""
      //   )
      //   .then(function(response) {
      //     // handle success
      //      this.combinedAnswersArray = ratedResponse.data.items
      //   })
      //   .catch(function(error) {
      //     // handle error
      //     console.log(error);
      //   });
    },
    createObjectMap() {
      //Map the comments,questions and answers all the one post id
    }
  }
};
</script>