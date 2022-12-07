<template>
    <div>
        <form>
                <h3 v-if="check">회원조회/수정</h3>
                <h3 v-else>회원가입</h3>
                ID: <input type="text" v-model="id" id="id" :readonly="check"><br>
                PW: <input type="text" v-model="password" id="password"><br>
                이름: <input type="text" v-model="name" id="name"><br>
                권한: <select v-model="role">
                        <option value="default" disabled>::선택::</option>
                        <option value="User">일반유저</option>
                        <option value="Admin">관리자</option>
                    </select><br>
                <button type="button" @click="userUpdate" v-if="check">수정</button>
                <button type="button" @click="userInsert" v-else>등록</button>
                <button type="button" @click="reset">취소</button>
            </form>
            <br><br>
                <table border="1">
                    <thead>
                        <tr>
                            <th>id</th><th>password</th><th>name</th><th>role</th><th>조회</th><th>삭제</th>
                        </tr>
                    </thead>
                    <tbody>
                        <template v-for="u in users">
                            <tr>
                                <td>{{u.id}}</td>
                                <td>{{u.password}}</td>
                                <td>{{u.name}}</td>
                                <td>{{u.role}}</td>
                                <td><button type="button" @click="userSelect(u)" :disabled="check">조회</button></td>
                                <td><button type="button" @click="userDelete(u.id)">삭제</button></td>
                            </tr>
                        </template>
                    </tbody>
                </table>
    </div>
</template>
  
  <script>
  export default {
    name: 'IncreView',
    data(){
                return{
                    role:'default',
                    id:'',
                    name:'',
                    password:'',
                    check:false,
                    users:[],
                    emps:[],
                }
            },
            created(){ //데이터 땡겨올때 이거쓰면댐 굳굳
                let url='http://localhost/myserver/empSelect';
                fetch(url)
                .then(res=>res.json())
                .then(res=> {
                    this.emps = res;
                });

                url='http://localhost/myserver/userSelectAll';
                fetch(url)
                .then(res=>res.json())
                .then(res=> {
                    this.users = res;
                });
            },
            methods:{
                //UserController에서 @PostMapping -> @GetMapping으로 전부 변경

                //등록
                userInsert(){
                    if(this.role=='default'){
                        alert('권한을 선택해주세요');
                        return;
                    }
                    let url = `http://localhost/myserver/userInsert?id=${this.id}&password=${this.id}&name=${this.name}&role=${this.role}`;
                    fetch(url)
                    .then(res => res.json())
                    .then(res => {
                        this.users.push(res);
                    });
                },

                //삭제
                userDelete(userId){
                    let url = `http://localhost/myserver/userDelete?id=${userId}`;
                    fetch(url)
                    .then(res => res.json())
                    .then(res => {
                        let newU=[];
                        for(u of this.users){
                            if(u.id!=userId){
                                newU.push(u);
                            }
                        }
                        this.users=newU;
                    });
                },

                //조회
                userSelect(user){
                    this.check = true;
                    this.id=user.id;
                    this.name=user.name;
                    this.password=user.password;
                    this.role = user.role;
                },

                //수정
                //UserController의 userUpdate의 @RequestBody삭제
                //=>public UserVO userUpdate(UserVO vo) {mapper.merge(vo);return vo;}
                userUpdate(){
                    let url = `http://localhost/myserver/userUpdate?id=${this.id}&password=${this.id}&name=${this.name}&role=${this.role}`;
                    fetch(url)
                    .then(res => res.json())
                    .then(res => {
                        for(u of this.users){
                            if(u.id==res.id){
                                u.name = res.name;
                                u.password = res.password;
                                u.role = res.role;
                            }
                        }
                        this.check = false;
                        this.reset();
                    });
                },

                //초기화
                reset(){
                    this.check = false;
                    this.id='';
                    this.name='';
                    this.password='';
                    this.role = 'default';
                }
            }
  }
  </script>
  
  <style>
  
  </style>
  