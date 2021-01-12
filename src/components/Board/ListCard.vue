<template>
    <v-card
        outlined
        min-height="55"
        elevation="7"
        color="white"
        block
        class="ma-2 pa-3"
        min-width="100">
            <v-card-subtitle> {{card_name}} </v-card-subtitle>
    </v-card>
</template>
<script>
export default {
    props: ['card_id', 'card_name', 'card_description', 'card_order', 'card_archived', 'card_term']
}
</script>
