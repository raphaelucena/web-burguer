<template>
    <div class="message-container">
        {{ msg }}
    </div>
</template>

<script>
export default{
    name: "Message",
    props: {
        msg: String
    }
}
</script>

<style scoped>
    .message-container {
        color: #004085;
        background-color: #CCE5FF;
        border: 2px solid #88DAFF;
        border-radius: 5px;
        padding: 10px;
        margin: 30px auto;
    }
</style>