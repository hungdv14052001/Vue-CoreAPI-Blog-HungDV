<template>
    <div id="List">
        <div class="list-header">
            <div class="list-title">List Blog</div>
        </div>
        <div id="listBlog">
            <div class="title-row">
                <div class="title-id">
                    id
                </div>
                <div class="title-Tin">
                    Tin
                </div>
                <div class="title-type">
                    Loại
                </div>
                <div class="title-status">
                    Trạng thái
                </div>
                <div class="title-location">
                    Vị trí
                </div>
                <div class="title-date_public">
                    Ngày Public
                </div>
                <div class="title-edit">
                    Edit
                </div>
                <div class="title-delete">
                    Delete
                </div>
            </div>
            <ul class="list-Blogs">
                <li v-for="blog in listBlog" :key="blog.id">
                    <div class="data-row">
                        <div class="data-id">
                            {{blog.id}}
                        </div>
                        <div class="data-Tin">
                            {{blog.title}}
                        </div>
                        <div class="data-type">
                            {{blog.category}}
                        </div>
                        <div class="data-status">
                            {{isPublic(blog.isPublic)}}
                        </div>
                        <div class="data-location">
                            {{location(getListPositionById(blog.listPositionId))}}
                        </div>
                        <div class="data-date_public">
                            {{blog.datePublic.slice(0,-9)}}
                        </div>
                        <div class="data-edit">
                            <NuxtLink class="btn-Edit" :to="{ name: 'CreateEditPage', params: { id: blog.id}}">Edit</NuxtLink>
                        </div>
                        <div class="data-delete">
                            <button type="button" class="btn-Delete" @click="confirmDelete(blog.id)">Delete</button>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
    import mainJs from '../../assets/js/main.js';
    export default{
        props : ['listBlog', 'listPosition'],
        methods: {

            /// <summary>
            /// method isPublic (a) to get value of public by String
            /// </summary>
            /// <param name="publicByBool"></param>
            /// <returns>string of public</returns>
            isPublic (publicByBool) {
                if(publicByBool==true){
                    return "Yes";
                }
                else{
                    return "No";
                }
            },

            /// <summary>
            /// method location ( listPosition ) to get position of blog by String
            /// </summary>
            /// <param name="listPosition"></param>
            /// <returns>string of position</returns>
            location (listPosition) {
                var listLocation=["Việt Nam", "Châu Á", "Châu Âu", "Châu Mỹ"];
                var position = "";
                for(var i= 0; i<listPosition.length; i++){
                    position += listLocation[listPosition[i]-1]+" ";
                }
                return position;
            },

            /// <summary>
            /// get list position by  listPositionId
            /// </summary>
            /// <param name="id"></param>
            /// <returns>list of position</returns>
            getListPositionById(id){
                var listPositionById=[];
                for(var i=0; i< this.listPosition.length; i++){
                    if(this.listPosition[i].listPositionId==id){
                        listPositionById[listPositionById.length]=this.listPosition[i].positionId;
                    }
                }
                return listPositionById;
            },

            /// <summary>
            /// function confirmDelete(id) to confirm delete Blog 
            /// </summary>
            /// <param name="id"></param>
            /// <returns>response.json()</returns>
            confirmDelete(id) {
                if (confirm('Do you want to delete this Blog?')) {
                    this.dellBlog(id);
                    this.$router.push("/");
                } else {
                    alert("Cancelled");
                }
            },

            /// <summary>
            /// function dellBlog(id) to delete data from API
            /// </summary>
            /// <param name="url"></param>
            /// <param name="data"></param>
            /// <returns>response.json()</returns>
            dellBlog(id){
                var option ={
                    method: 'DELETE',
                    header:{
                        'Content-Type' : 'application/json'
                    },
                };
                fetch('https://localhost:44302/api/Blogs/'+id, option)
                    .then(function(response) {
                        response.json();
                    });
                location.reload();
                alert("successful delete!");
            }
        }
    }
</script>