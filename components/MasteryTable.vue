<template>
    <div>
        <b-table :data="data" :columns="columns" selectable @select="selected"></b-table>
    </div>
</template>
<script>


export default {
    name: 'MasteryTable',    
    props: {
        championMastery: {},
        selectable: true,
    },
    data(){
        return{
            data: this.championMastery,
            
                columns: [
                    {
                        field: 'id',
                        label: 'ID',
                        width: '40',
                    },
                    {
                        field: 'name',
                        label: 'Name',
                    },
                    {
                        field: 'level',
                        label: 'Level',
                    },
                    {
                        field: 'points',
                        label: 'Points',
                    },
                    {
                        field: 'lastPlayTime',
                        label: 'LastTimePlayed',
                    },
                    {
                        field: 'chestGranted',
                        label: 'Chest Granted',
                    },
                ]
        }
    },
    methods: {
            selected(item){
                this.$router.push({path: '/championDetails/' + item.id});
            }
        }
    
}
</script>
