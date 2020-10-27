<template>
    <div class="user_twit draggable" draggable="true" @dragstart="dragStart" @dragend="dragEnd">
        @{{username}} - {{twit.content}}
    </div>
    
</template>

<script>
export default {
    name: "Twit",
    props: {
        username: {
            type: String,
            required: true
        },
        twit: {
            type: Object,
             required: true
        }
    },
    methods: {
        dragStart(e){
            e.target.classList.add('dragging');
        },
        dragEnd(e){
            e.target.classList.remove('dragging');
            this.$emit('drageventended', e.target);
        }
    }
}
</script>

<style scoped>
    .user_twit{
        background: white;
        padding: 1rem;
        box-shadow: 0px 0px 4px rgb(189, 189, 189);
        margin-bottom: .5rem;
    }
    .user_twit:hover{
        cursor: pointer;
        transform: scale(1.02);
    }
    .dragging{
        opacity: 0.5;
    }
</style>