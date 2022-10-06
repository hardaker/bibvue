<script>
 var vuedata = {
     'bib_files': [
         { 'file': 'wjh-presentations.json',
           'title': 'presentations'},
         { 'file': 'wjh-rssac.json',
           'title': 'rssac publications'},
         { 'file': 'wjh-rfc.json',
           'title': 'IETF rfcs'},
         { 'file': 'wjh-software.json',
           'title': 'software'},
         { 'file': 'wjh-bib.json',
           'title': 'Peer-reviewed papers'},
     ],
     'sections': {
         'Peer-reviewed papers': {},
         'presentations': {},
         'rssac publications': {},
         'IETF rfcs': {},
         'software': {},
     },
     'always_show': {
         'url': true,
         'where': true,
         'date': false,
         'authors': false,
         'reference': false,
         'note': false,
         'keywords': false,
         'abstract': true,
     },
     'bibsdir': "bibs",

     // don't need to touch these
     'current_section': 0,
     expanded: false,
 }

 var transform_id = function(name) {
     name.replaceAll(/[^a-zA-Z0-9]/g, "_");
     return vuedata['bibsdir'] + "/" + name + ".bib";
 }

 export default {
     name: 'BibVue',
     props: [],

     created() {
         for(var content in vuedata.bib_files) {
             vuedata['sections'][vuedata.bib_files[content].title] = {}
         }
         load_bibs()
     },
     data: function() {
         return vuedata;
     },
     methods: {
         'showdetails': function() {
             console.log("expand")
         },
         'transform_id': transform_id,
     }
 }

 var load_bibs = async function() {
     for (var i=0; i < vuedata.bib_files.length ; i++) {
         try {
             const response =
                 await fetch(vuedata.bib_files[i].file);
             const data = await response.json();

             for(var j=0; j < data.length; j++) {
                 if (data[j]['id']) {
                     data[j]['transformed_id'] = transform_id(data[j]['id']);
                     console.log("converted " + data[j]['id'] + " to " + data[j]['transformed_id']);
                 } else {
                     data[j]['transformed_id'] = undefined;
                     console.log("no id for: ");
                     console.log(data[j]);
                 }
             }

             vuedata['sections'][vuedata.bib_files[i].title] =
                 { 'title': vuedata.bib_files[i].title,
                   'content': data}
             console.log("fetched "+ vuedata.bib_files[i].file)

         } catch (error) {
             console.log('failed to fetch ' + vuedata.bib_files[i].file);
             console.log(error);
         }
     }
 }
</script>
<template>
    <v-container fluid>
        <v-card>
            <v-card-title>
                Publications
            </v-card-title>
            <v-card-text>
                <v-btn class="float-right" @click="expanded = !expanded">
                    <span v-if="expanded">
                        Hide Details
                    </span>
                    <span v-if="!expanded">
                        Show Details
                    </span>
                </v-btn>
                <v-tabs v-model="current_section">
                    <v-tab v-for="section in sections"
                           :key="section.title">
                        {{section.title}}
                    </v-tab>
                    
                    <v-tabs-items v-model="current_section">
                        <v-tab-item v-for="section in sections"
                                    :key="section.title">
                            <span v-for="item in section.content" :key="item.id">
                                <h3>{{item.title}}
                                    <a class="float-right" :href="item.transformed_id">[cite]</a>
                                </h3>
                                <v-simple-table dense>
                                    <template v-slot:default>
                                        <tbody>
                                            <tr>
                                                <th width="20%">cite</th>
                                                <td><a :href="item.transformed_id">[cite]</a></td>
                                            </tr>
                                            <tr v-if="item.URL">
                                                <th width="20%">URL</th>
                                                <td><a :href="item.URL">{{item.URL}}</a></td>
                                            </tr>
                                            <tr v-if="item.issued['date-parts'] && (expanded || always_show['date'])">
                                                <th width="20%">Date</th>
                                                <td>{{item.issued['date-parts'].join("/").replaceAll(",","/")}}</td>
                                            </tr>
                                            <tr v-if="(item['collection-title'] || item['container-title'] || item['publisher']) && (expanded || always_show['where'])">
                                                <th width="20%">Where</th>
                                                <td>
                                                    <span v-if="item['container-title']">{{item['container-title']}}, </span>
                                                    <span v-if="item['collection-title']">{{item['collection-title']}}, </span>
                                                    <span v-if="item['publisher']">{{item.publisher}}</span></td>
                                            </tr>
                                            <tr v-if="item.author && (expanded || always_show['authors'])">
                                                <th width="20%">Authors</th>
                                                <td>
                                                    <span :key="author.given " v-for="author in item.author">
                                                        {{author.given}} {{author.family}},
                                                    </span>
                                                </td>
                                            </tr>
                                            <tr v-if="(item.DOI || item.ISBN) && (expanded || always_show['reference'])">
                                                <th width="20%">Reference</th>
                                                <td>{{item.DOI}} / {{item.ISBN}}</td>
                                            </tr>
                                            <tr v-if="item.note && (expanded || always_show['note'])">
                                                <th width="20%">Notes</th>
                                                <td>{{item.note}}</td>
                                            </tr>
                                            <tr v-if="item.keyword && (expanded || always_show['keywords'])">
                                                <th width="20%">Keywords</th>
                                                <td>{{item.keyword}}</td>
                                            </tr>
                                        </tbody>
                                    </template>
                                </v-simple-table>
                                <p v-if="item.abstract && (expanded || always_show['abstract'])"
                                   class="pl-6">
                                    <strong>Abstract: </strong>
                                    {{item.abstract}}
                                </p>
                            </span>
                        </v-tab-item>
                    </v-tabs-items>

                </v-tabs>
            </v-card-text>
        </v-card>

    </v-container>
</template>
