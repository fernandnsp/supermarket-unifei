<template>
    <div>
        <Header :route="route" />
        <Signup />
    </div>
</template>

<script>
import Header from '~/components/Header'
import Signup from '~/components/Signup'

export default {
    name: 'IndexPage',

    components:{
        Header,
        Signup,
    },

    data(){
        return {
            route: {
                link: '/',
                name: 'Table',
            },
        }
    }
}
</script>