<template>
    <div>
        <b-navbar toggleable="sm" class="shadow-sm">

            <b-navbar-brand class="">QuizMo</b-navbar-brand>
            <b-navbar-nav>
                <b-nav-text class="text-primary">Counter: {{numCorrect}}/{{numTotal}}</b-nav-text>
            </b-navbar-nav>
            <hr>
            <div class="float-right text-right">
                <small class="text-secondary">Category: Science and nature</small>
            </div>
        </b-navbar>
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