<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>SmartUp Quiz</b></b-nav-item>
            <b-nav-item disabled >Score {{numCorrect}}/{{numTotal}}</b-nav-item>

            <b-nav-item disabled><b>Total Questions</b></b-nav-item>
            <b-nav-item disabled >{{question_counted}}/{{questions_no}}</b-nav-item>
        </b-nav>
    </div>
</template>


<script>
export default {
    props:[
        'numCorrect',
        'numTotal',
        'questions_no',
        'question_counted'
    ]
}
</script>