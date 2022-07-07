<template>
    <div id="CreateEdit">
        <div class="CreateEdit-header">
            <div class="CreateEdit-title">News Blog</div>
        </div>
        <div class="CreateEdit-contairner">
            <label for="" class="lb">Tiêu đề: </label>
            <input type="text" class="txtTieuDe" v-model="Title">
            <label for="" class="lb">Mô tả ngắn:</label>
            <textarea class="txtMota" id="txtDes" rows="4" v-model="Des"></textarea>
            <label for="" class="lb">Chi tiết:</label>
            <textarea class="txtChitiet" id="txtDetail" rows="8" v-model="Detail"></textarea>
            <label for="" class="lb">Hình ảnh:</label>
            <div class="txtTieuDe">
                <input type="file" class="input-file" name="myfile" ></input>
            </div>
            <label for="" class="lb">Vị trí:</label>
            <div class="txtTieuDe">
                <input type="checkbox" class="cbLocation" value="1" v-model="Position"><label for="cbVietNam">Việt Nam</label>
                <input type="checkbox" class="cbLocation" value="2" v-model="Position"><label for="cbChauA">Châu Á</label>
                <input type="checkbox" class="cbLocation" value="3" v-model="Position"><label for="cbChauAu">Châu Âu</label>
                <input type="checkbox" class="cbLocation" value="4" v-model="Position"><label for="cbChauMy">Châu Mỹ</label>
            </div>
            <label for="" class="lb">Public:</label>
            <div class="txtTieuDe">
                <input type="radio" class="cbLocation" v-model="IsPublic" :value="true" />Yes
                <input type="radio" class="cbLocation" v-model="IsPublic" :value="false" />No
            </div>
            <div class="type-contairner">
                <div class="type-item">
                    <label >Loại:</label>
                </div>
                <div class="type-item">
                    <label >Date Public:</label>
                </div>
            </div>
            <div class="type-contairner">
                <div class="type-item">
                    <select  v-model="Category" class="txtType">
                        <option v-for="category in Categorys" v-model="category.id">{{category.name}}</option>
                    </select>
                </div>
                <div class="type-item">
                    <input v-model="DatePublic" type="text" class="txtType" placeholder="yyyy/mm/dd">
                </div>
            </div>
        </div>
        <div class="footer-CreateEdit">
            <button type="button" class="btnSubmit" @click="submit">Submit</button>
            <button type="button" class="btnClear" @click="clear">Clear</button>
        </div>
    </div>
</template>
<script>
import EventBus from '../../assets/js/main.js';
const listCategory= [{id: 0, name: "Kinh tế"}, {id: 1, name: "Thế Giới"}, {id: 2, name: "Chính Trị"}, {id: 4, name: "Chính Trị"}, {id: 3, name: "Thời Sự"}, {id: 4, name: "Giải Trí"}, {id: 5, name: "Kinh Doanh"}, {id: 6, name: "Giáo Dục"}, {id: 7, name: "Thể Thao"}, {id: 8, name: "Thể Thao"}, {id: 9, name: "Thể Thao"}, {id: 10, name: "Thể Thao"}, {id: 11, name: "Thể Thao"}];
const apiBlog= 'https://localhost:44302/api/Blogs?id=';
const apiPositionItem="https://localhost:44302/api/PositionItems";
export default { 
    /// <summary>
    /// functon fetch() to set current Blog
    /// </summary>
    /// <returns></returns>
    async beforeMount(){
        try{
            if(this.currentId!='0'){
                var listBlog= await fetch(apiBlog+this.currentId)
                    .then((res)=>res.json());
                var blog=listBlog.find(x=>x.id===this.currentId);
                this.Title= blog.title;
                this.Des= blog.des;
                this.Detail= blog.detail;
                this.DatePublic= blog.datePublic.slice(0, -9);
                this.Category= this.Categorys.find(x=>x.id===blog.category).name;
                this.ListPositionId= blog.listPositionId;
                this.IsPublic= blog.isPublic;
                this.listPosition = await fetch(apiPositionItem)
                    .then((res)=>res.json());
                this.Position= this.getListPositionById(this.ListPositionId);
            }
        }
        catch{

        }
    },
    data(){
        return{
            Title: '',
            Des:'',
            Detail:'',
            DatePublic:'',
            Category: '',
            Position:[],
            IsPublic: true,
            currentId: this.$route.params.id,
            Categorys: listCategory,
            listPosition:[],
            ListPositionId: 0,
        };
    },
    methods: {

        /// <summary>
        /// method submit() submit Blog into API
        /// </summary>
        submit(){
            if(this.Title.length==0||this.Des.length==0||this.Detail.length==0||this.DatePublic.length==0){
                    alert("Vui lòng nhập đầy đủ thông tin");
                return;
            }
            try{
                var date= new Date(this.DatePublic);
            }
            catch{
                alert("Thông tin Date Public không hợp lệ!");
                return;
            }
            var dataBlog={
                    title: this.Title,
                    des: this.Des, 
                    detail: this.Detail, 
                    category: this.Categorys.findIndex(x=>x.name===this.Category), 
                    isPublic: this.IsPublic, 
                    datePublic: new Date(this.DatePublic), 
                    listPositionId: this.ListPositionId, 
                    thumbs: "amess.jpg"
            };
            if(this.currentId== 0){
                this.create(dataBlog);
            }
            else if(this.currentId!=0){
                this.edit(dataBlog);
            }
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
        /// method create(dataBlog) Create new Blog in Api
        /// </summary>
        /// <param name="dataBlog"></param>
        create(dataBlog){
            this.postData('https://localhost:44302/api/Blogs', dataBlog)
                    .then(data => {
                        this.currentId= data.id;
                        console.log(this.currentId);
                    });
            for(var i = 0; i< this.Position.length; i++){
                var positionItem={
                    listPositionId: this.currentId,
                    positionId: this.Position[i], 
                };
                this.postData('https://localhost:44302/api/PositionItems', positionItem)
                    .then(data=>{});
            }
            this.$router.push("/");
        },

        /// <summary>
        /// function postData(url = '', data = {}) to post data into API
        /// </summary>
        /// <param name="url"></param>
        /// <param name="data"></param>
        /// <returns>response.json()</returns>
        async postData(url = '', data = {}) {
            const response = await fetch(url, {
                method: 'POST',
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(data) 
            });
            return response.json(); 
        },

        /// <summary>
        /// method edit(dataBlog) edit current Blog in Api
        /// </summary>
        /// <param name="dataBlog"></param>
        /// <returns></returns>
        async edit(dataBlog){
            this.putData('https://localhost:44302/api/Blogs/'+this.currentId, dataBlog)
                    .then(data => {
                        alert("Successful PUT!");
                    });
            var list = await fetch('https://localhost:44302/api/PositionItems/listPositionId='+this.currentId)
                .then((res)=> res.json());
            for(var i=0; i<list.length; i++){
                this.dellPositionItem(list[i].id);
            }
            for(var i = 0; i< this.Position.length; i++){
                var positionItem={
                    listPositionId: this.currentId,
                    positionId: this.Position[i], 
                };
                this.postData('https://localhost:44302/api/PositionItems', positionItem)
                    .then(data=>{});
            }
            this.$router.push("/");
        },

        /// <summary>
        /// delete PositionItem by Id
        /// </summary>
        /// <param name="id"></param>
        /// <returns>response</returns>
        dellPositionItem(id){
            var option ={
                method: 'DELETE',
                header:{
                    'Content-Type' : 'application/json'
                },
            };
            fetch('https://localhost:44302/api/PositionItems/'+id, option)
                .then(function(response) {
                    response.json();
                });
        },

        /// <summary>
        /// function putData(url = '', data = {}) to put data in API
        /// </summary>
        /// <param name="url"></param>
        /// <param name="data"></param>
        /// <returns>response.json()</returns>
        async putData(url = '', data = {}) {
            const response = await fetch(url, {
                method: 'PUT',
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(data) 
            });
            return response.json();
        },

        /// <summary>
        /// method clear() to clear every control in this component
        /// </summary>
        /// <param name="dataBlog"></param>
        clear(){
            this.Title= '';
            this.Des='';
            this.Detail='';
            this.Data_pubblic='';
            this.Category= '';
            this.Position=[];
            this.Public= true;
            this.currentId= 0;
        }
    }
}
    
</script>
