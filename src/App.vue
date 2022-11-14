<template>
  <main>
    <div class="mx-5 my-5 card">
        <div class="card-header d-flex justify-content-between align-items-center py-3">
            <h4 class="m-0">DCIT24 Midterm Reviewer</h4>
        </div>
        <div class="card-body">
            <div v-if="message" class="text-white p-3 rounded mb-3" :class="[ message == 'correct' ? 'bg-success' : 'bg-danger' ]">
                <p class="m-0">{{ message == "correct" ? "Congratulation! Your answer is correct." : message }}</p>
            </div>
            <div class="mb-3">
                <label for="answer" class="form-label"><strong class="text-danger fs-2">Q.</strong> <span class="question fs-3">{{ question }}</span></label>
                <input type="text" class="form-control mt-2" v-model="user_answer" placeholder="Type your answer" aria-label="answer">
            </div>
        </div>
        <div class="card-footer d-flex justify-content-end align-items-center py-3">
            <button v-if="!isSubmitted" class="btn btn-primary" @click="verifyAnswer()">Submit Answer</button>
            <button v-else class="btn btn-success" @click="nextQuestion()">Next Question</button>
        </div>
    </div>
  </main>
</template>

<script>
    export default {
        mounted() {
            this.nextQuestion();
        },
        data() {
            return {
                count: 0,
                message: null,
                question: "",
                answer: "",
                user_answer: "",
                isSubmitted: false,
                questions: [
                    {
                        question: "The example of this data are video, sounds, photographic images, documents, Maps.",
                        answer: "unstructured data/multimedia data"
                    },
                    {
                        question: "This schema combines the different external views into a single, coherent definition of the enterprise's data. It represents the view of the data architect or data administrator.",
                        answer: "conceptual schema"
                    },
                    {
                        question: "Stored representations of objects and events that have meaning and importance in the user's environment.",
                        answer: "data"
                    },
                    {
                        question: "A database that represents data as a collection of tables in which all data relationships are represented by common values in related tables.",
                        answer: "relational database"
                    },
                    {
                        question: "More detailed and more closely match the way a database and applications against it.",
                        answer: "project-level data model"
                    },
                    {
                        question: "Data that have been processed in such a way as to increase the knowledge of the person who uses the data.",
                        answer: "information"
                    },
                    {
                        question: "A graphical model that shows the high-level entities for the organization and the relationships among those entities.",
                        answer: "enterprise data model"
                    },
                    {
                        question: "An organized collection of logically related data.",
                        answer: "database"
                    },
                    {
                        question: "This is the view of managers and other employees who are the database users. It can be represented as a combination of the enterprise data model.",
                        answer: "external schema"
                    },
                    {
                        question: "Graphical systems used to capture the nature and relationships among data. Data models are created at both the enterprise and project levels.",
                        answer: "data models"
                    },
                    {
                        question: "The first step in database development, in which the scope and general contents of organizational databases are specified.",
                        answer: "enterprise data modeling"
                    },
                    {
                        question: "Data that describe the properties or characteristics of end-user data, and the context of that data.",
                        answer: "metadata"
                    },
                    {
                        question: "A software system that is used to create, maintain, and provide controlled access to user databases.",
                        answer: "database management systems"
                    },
                    {
                        question: "A formal, top-down methodology that uses a data orientation to create and maintain information systems.",
                        answer: "information engineering"
                    },
                    {
                        question: "A conceptual blueprint or plan that expresses the desired future structure for the information systems in an organization.",
                        answer: "information systems architecture"
                    },
                    {
                        question: "The example of this data are numeric, character and dates.",
                        answer: "structured data"
                    },
                    {
                        question: "Consists of logical and physical schema. The logical schema is the representation of data for a type of data management technology.",
                        answer: "internal schema"
                    },
                    {
                        question: "It occurs when the project is naturally or unnaturally terminated.",
                        answer: "project close down"
                    },
                    {
                        question: "A planned undertaking of related activities to reach an objective that has a beginning and an end.",
                        answer: "project"
                    },
                    {
                        question: "Collection of entities that have a common properties/ characteristic.",
                        answer: "entity type"
                    },
                    {
                        question: "A property or characteristics of an entity type.",
                        answer: "attributes"
                    },
                    {
                        question: "An attribute that uniquely identifies individual instances of an entity type.",
                        answer: "identifier"
                    },
                    {
                        question: "A person, a place, object, event or concpets in the user environment about which the organization wishes to maintain data.",
                        answer: "entity"
                    },
                    {
                        question: "A tool for communications between database designers and end users during the analysis phase of database development.",
                        answer: "er model"
                    },
                    {
                        question: "A graphical representation of an E-R model.",
                        answer: "e-r diagram"
                    },
                    {
                        question: "An entity type whose existence depends on some other entity type.",
                        answer: "weak entity"
                    },
                    {
                        question: "An attribute that may take on more than one value for agiven entity instance.",
                        answer: "multivalued attribute"
                    },
                    {
                        question: "An association between two or more terms.",
                        answer: "fact"
                    },
                    {
                        question: "A meaningful association between (or among) entity types.",
                        answer: "relationship types"
                    },
                    {
                        question: "A single occurrence of an entity type.",
                        answer: "entity instance"
                    },
                    {
                        question: "An explanation of a term or a fact.",
                        answer: "data definition"
                    },
                    {
                        question: "An attribute that can be broken down into component parts.",
                        answer: "composite attribute"
                    },
                    {
                        question: "An identifier that consist of a composite attribute.",
                        answer: "composite identifier"
                    },
                    {
                        question: "A statement that define or constrains some aspect of the business. It is intended to assert business structure or to control or influence the behavior of the business.",
                        answer: "business rule"
                    },
                    {
                        question: "An association among the instances of one or more entity types that is of interest to the organization.",
                        answer: "relationships"
                    },
                    {
                        question: "An attribute that cannot broken down into smaller components.",
                        answer: "simple attribute"
                    },
                    {
                        question: "A word or phrase that has a specific meaning for the business",
                        answer: "term"
                    },
                    {
                        question: "An attribute whose values can be calculated from related attribute values",
                        answer: "derived attribute"
                    },
                    {
                        question: "One that exists independently of other entity type",
                        answer: "strong entity"
                    }
                ]
            }
        },
        methods: {
            verifyAnswer() {
                if(this.user_answer == "") this.message = "Please put your answer";
                else {
                    if(this.user_answer == this.answer) this.message = "correct";
                    else this.message = "The correct answer is: " + this.answer;

                    this.isSubmitted = true;
                }
            },
            nextQuestion() {
                this.isSubmitted = false;
                this.message = null;
                this.user_answer = "";

                const SELECTED_QUESTION = this.questions[Math.floor(Math.random() * this.questions.length)];
                const QUESTION = SELECTED_QUESTION.question;
                const ANSWER = SELECTED_QUESTION.answer;

                this.question = QUESTION;
                this.answer = ANSWER;
            }
        }
    }
</script>