<template>
    <div class="page-success" style="margin-top:80px; margin-bottom:292px;">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Thank you</h1>

                <p>Your order will be processed within 48 hours</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = 'Success | VOTL'
        
/* This refreshes the success page so that the cart 
count will reset back to 0 after submitting an order to Stripe */
        if (localStorage.getItem('reloaded')) {
            localStorage.removeItem('reloaded');
        } else {
        localStorage.setItem('reloaded', '1');
        location.reload();
        vm.$forceUpdate();
        } 
    },
}
</script>