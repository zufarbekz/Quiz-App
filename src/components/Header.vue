<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><strong> Fancy Quiz App </strong></b-nav-item>
            <b-nav-item disabled>{{numCorrect}}/{{numTotal}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ]

    
}
</script>