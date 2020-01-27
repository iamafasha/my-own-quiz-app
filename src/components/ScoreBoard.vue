<template >
    <div>
        <br/>
        <h1>Your score was</h1>
        <h2>{{result_percentage}}%</h2>
    </div>
</template>

<script>
export default {
    props:[
        'numCorrect',
        'numTotal',
        'questions_no'
    ],
    computed: {
        result_percentage(){
            if(this.numTotal!=0){
            return ((this.numCorrect/this.numTotal)*100)
            }
            return 0;
        }
    },
}
</script>