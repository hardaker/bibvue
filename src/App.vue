<template>
    <v-app>
        <v-main>
            <PersonalHeader/>
            <BibVue/>
            <Trailer/>
        </v-main>
    </v-app>
</template>

<script>
 import BibVue from './components/BibVue';
 import PersonalHeader from './components/PersonalHeader';
 import Trailer from './components/Trailer';

 export default {
     name: 'App',

     components: {
         BibVue,
         PersonalHeader,
         Trailer,
     },

     data: () => ({
         //
     }),
 };
</script>
