<template>
  <main>
    <div class="mx-5 my-5 card">
        <div class="card-header d-flex justify-content-between align-items-center py-3">
            <h3 class="m-0">Score Streak: {{ score }}</h3>
        </div>
        <div class="card-body">
            <div v-if="message" class="text-white p-3 rounded mb-3" :class="[ message == 'correct' ? 'bg-success' : 'bg-danger' ]">
                <p class="m-0">{{ message == "correct" ? "Congratulation! Your answer is correct." : message }}</p>
            </div>
            <div class="mb-3">
                <label for="answer" class="form-label"><strong class="text-danger fs-2">Q.</strong> <span class="question fs-3">{{ question }}</span></label>
                <input 
                    type="text" 
                    class="form-control mt-2" 
                    v-model="user_answer" 
                    placeholder="Type your answer" 
                    aria-label="answer" 
                    @keyup.enter="verifyAnswer()" 
                    :disabled="isSubmitted">
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
            this.questions = this.questions.sort((a, b) => 0.5 - Math.random());
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
                currentIndex: 0,
                score: 0,
                questions: [
                    {
                        question: "The example of this data are video, sounds, photographic images, documents, maps.",
                        answer: "Unstructured Data/Multimedia Data"
                    },
                    {
                        question: "This schema combines the different external views into a single, coherent definition of the enterprise's data. It represents the view of the data architect or data administrator.",
                        answer: "Conceptual Schema"
                    },
                    {
                        question: "Stored representations of objects and events that have meaning and importance in the user's environment.",
                        answer: "Data"
                    },
                    {
                        question: "A database that represents data as a collection of tables in which all data relationships are represented by common values in related tables.",
                        answer: "Relational Database"
                    },
                    {
                        question: "More detailed and more closely match the way a database and applications against it.",
                        answer: "Project-Level Data Model"
                    },
                    {
                        question: "Data that have been processed in such a way as to increase the knowledge of the person who uses the data.",
                        answer: "Information"
                    },
                    {
                        question: "A graphical model that shows the high-level entities for the organization and the relationships among those entities.",
                        answer: "Enterprise Data Model"
                    },
                    {
                        question: "An organized collection of logically related data.",
                        answer: "Database"
                    },
                    {
                        question: "This is the view of managers and other employees who are the database users. It can be represented as a combination of the enterprise data model.",
                        answer: "External Schema"
                    },
                    {
                        question: "Graphical systems used to capture the nature and relationships among data.",
                        answer: "Data Models"
                    },
                    {
                        question: "The first step in database development, in which the scope and general contents of organizational databases are specified.",
                        answer: "Enterprise Data Modeling"
                    },
                    {
                        question: "Data that describe the properties or characteristics of end-user data, and the context of that data.",
                        answer: "Metadata"
                    },
                    {
                        question: "A software system that is used to create, maintain, and provide controlled access to user databases.",
                        answer: "Database Management Systems"
                    },
                    {
                        question: "A formal, top-down methodology that uses a data orientation to create and maintain information systems.",
                        answer: "Information Engineering"
                    },
                    {
                        question: "A conceptual blueprint or plan that expresses the desired future structure for the information systems in an organization.",
                        answer: "Information Systems Architecture"
                    },
                    {
                        question: "The example of this data are numeric, character and dates.",
                        answer: "Structured Data"
                    },
                    {
                        question: "Consists of logical and physical schema. The logical schema is the representation of data for a type of data management technology.",
                        answer: "Internal Schema"
                    },
                    {
                        question: "It occurs when the project is naturally or unnaturally terminated.",
                        answer: "Project Close Down"
                    },
                    {
                        question: "A planned undertaking of related activities to reach an objective that has a beginning and an end.",
                        answer: "Project"
                    },
                    {
                        question: "Collection of entities that have a common properties/characteristic.",
                        answer: "Entity Type"
                    },
                    {
                        question: "A property or characteristics of an entity type.",
                        answer: "Attributes"
                    },
                    {
                        question: "An attribute that uniquely identifies individual instances of an entity type.",
                        answer: "Identifier"
                    },
                    {
                        question: "A person, a place, object, event or concpets in the user environment about which the organization wishes to maintain data.",
                        answer: "Entity"
                    },
                    {
                        question: "A tool for communications between database designers and end users during the analysis phase of database development.",
                        answer: "ER Model"
                    },
                    {
                        question: "A graphical representation of an E-R model.",
                        answer: "E-R Diagram"
                    },
                    {
                        question: "An entity type whose existence depends on some other entity type.",
                        answer: "Weak Entity"
                    },
                    {
                        question: "An attribute that may take on more than one value for a given entity instance.",
                        answer: "Multivalued Attribute"
                    },
                    {
                        question: "An association between two or more terms.",
                        answer: "Fact"
                    },
                    {
                        question: "A meaningful association between (or among) entity types.",
                        answer: "Relationship Types"
                    },
                    {
                        question: "A meaningful association between (or among) entity instance.",
                        answer: "Relationship Instance"
                    },
                    {
                        question: "An application program (or set of related programs) that is used to perform a series of database activities (create, read, update, and delete) on behalf of database users.",
                        answer: "Database Application"
                    },
                    {
                        question: "The separation of data description from the application programs that used the data.",
                        answer: "Data Independence"
                    },
                    {
                        question: "A logical description of some portion of the database that is required by a user to perform some task.",
                        answer: "User View"
                    },
                    {
                        question: "Designed to support one user.",
                        answer: "Personal Database"
                    },
                    {
                        question: "A relatively small team of people who collaborate on the same project or application or on a group of similar projects or applications.",
                        answer: "Workgroup Database"
                    },
                    {
                        question: "Generally larger than a workgroup. Designed to support the various functions and activities of a department or division.",
                        answer: "Departmental/Divisional Database"
                    },
                    {
                        question: "One whose scope is the entire organization or enterprise. Such databases are intended to support organization wide operations and decision making.",
                        answer: "Enterprise Databases"
                    },
                    {
                        question: "A business management system that integrates all functions of the enterprise, such as manufacturing, sale, finance, marketing, inventory, accounting, and human resources.",
                        answer: "Enterprise Resource Planning Systems"
                    },
                    {
                        question: "An integrated decision support database whose content is derived from the various operational databases.",
                        answer: "Data Warehousing Implementations"
                    },
                    {
                        question: "A relationship between the instances of a single entity type.",
                        answer: "Unary"
                    },
                    {
                        question: "A relationship between the instances of a two entity type.",
                        answer: "Binary"
                    },
                    {
                        question: "A simultaneous relationship among the instance of 3 entity types",
                        answer: "Ternary"
                    },
                    {
                        question: "A single occurrence of an entity type.",
                        answer: "Entity Instance"
                    },
                    {
                        question: "An explanation of a term or a fact.",
                        answer: "Data Definition"
                    },
                    {
                        question: "An attribute that can be broken down into component parts.",
                        answer: "Composite Attribute"
                    },
                    {
                        question: "An identifier that consist of a composite attribute.",
                        answer: "Composite Identifier"
                    },
                    {
                        question: "A statement that define or constrains some aspect of the business. It is intended to assert business structure or to control or influence the behavior of the business.",
                        answer: "Business Rule"
                    },
                    {
                        question: "An association among the instances of one or more entity types that is of interest to the organization.",
                        answer: "Relationships"
                    },
                    {
                        question: "An attribute that cannot broken down into smaller components.",
                        answer: "Simple Attribute"
                    },
                    {
                        question: "A word or phrase that has a specific meaning for the business.",
                        answer: "Term"
                    },
                    {
                        question: "An attribute whose values can be calculated from related attribute values.",
                        answer: "Derived Attribute"
                    },
                    {
                        question: "One that exists independently of other entity type.",
                        answer: "Strong Entity"
                    }
                ]
            }
        },
        methods: {
            verifyAnswer() {
                if(this.user_answer == "") this.message = "Please put your answer";
                else {
                    if(this.user_answer.trim().toLowerCase() == this.answer.toLowerCase()) {
                        this.message = "correct";
                        this.score++;
                    } else {
                        this.message = "The correct answer is: " + this.answer;
                        this.score = 0;
                    }

                    this.isSubmitted = true;
                }
            },
            nextQuestion() {
                this.isSubmitted = false;
                this.message = null;
                this.user_answer = "";

                this.currentIndex++;

                if(this.currentIndex == this.questions) {
                    this.currentIndex = 0;
                    this.questions = this.questions.sort(() => 0.5 - Math.random());
                }

                const SELECTED_QUESTION = this.questions[this.currentIndex];
                const QUESTION = SELECTED_QUESTION.question;
                const ANSWER = SELECTED_QUESTION.answer;

                this.question = QUESTION;
                this.answer = ANSWER;
            }
        }
    }
</script>