<script>
import axios from 'axios';

export default {
  data() {
    return {
      nama: '',
      kasus: '',
      jenis_kelamin: '',
      users: [],
      selectedUser: null,
      createdUser: null,
      deleteduser: null
    }
  },
  created() {
    this.tampilkanData();
  },
  methods: {

    async tampilkanData() {
      try {
        const response = await axios.get('http://localhost:5000/users/');
        this.users = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    async buatData(event) {
      try {
        // prevent form submission
        event.preventDefault();

        const response = await axios.post('http://localhost:5000/users/', {
          nama: this.nama,
          kasus: this.kasus,
          jenis_kelamin: this.jenis_kelamin
        });
        this.createUser= response.data;
        this.nama = '';
        this.kasus = '';
        this.jenis_kelamin = '';
        window.location.reload()
      } catch (error) {
        console.error(error);
      }
    },
    async updateData(id) {
      try {
        const response = await axios.patch(`http://localhost:5000/users/${id}`, {
          nama: this.nama,
          kasus: this.kasus,
          jenis_kelamin: this.jenis_kelamin
        });
        this.selectedUser = response.data;
      } catch (error) {
        console.error(error);
      }
      // Membersihkan data yang tersimpan di properti data selectedUser
      this.selectedUser = null;
    },
    async hapusData(id) {
      try {
        await axios.delete(`http://localhost:5000/users/${id}`);
       
        this.tampilkanData();
      } catch (error) {
        console.error(error);
      }
        
    },
    selectUser(user) {
      this.selectedUser = user;
    },

    createUser(users) {
      this.createdUser = users;
    }
  }
}
</script>

<template>

<button
            class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l"
              @click="createUser(users)">Create</button>   
       

  <div class="">
    <!-- TABLE MENAMPILKAN DATA -->
    <table class="">
      <thead>
        <tr>
          
          <th class="">Nama</th>
          <th class="">Kasus</th>
          <th class="">Jenis_kelamin</th>
          <th class="">Metode</th>
        
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
        
          <td class="">{{ user.nama }}</td>
          <td class="">{{ user.kasus }}</td>
          <td class="">{{ user.jenis_kelamin }}</td>
          <td class="">
            <button
              class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l"
              @click="selectUser(user)">Update</button>
              <button
                  class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l"
                  @click="hapusData(user.id)">Hapus Data</button>
          </td>
         
        </tr>
      </tbody>
    </table>
  </div>

  <div class="">
    <div class="">
      <table v-if="createdUser" 
      class="">
        <thead class="">
          <th scope="col" class="">
            <tr>
              <td>Nama: </td>
              <td>
                <input
                  class=""
                  type="text" id="nama" v-model= "nama" />
              </td>
            </tr>
          </th>
          <th scope="col" class="">
            <tr>
              <td>Kasus:</td>
              <td>
                <input
                  class=""
                  type="kasus" id="kasus" v-model= "kasus" />
              </td>
            </tr>
          </th>
          <th scope="col" class="">
            <tr>
              <td>Jenis_kelamin:</td>
              <td>
                <input
                  class=""
                  type="jenis_kelamin" id="jenis_kelamin" v-model= "jenis_kelamin" />
              </td>
            </tr>
          </th>
          <th scope="col" class="">
            <tr>
              <td colspan="2">
                <button
                  class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l"
                  @click.prevent="buatData" type="submit">Buat Data</button>
              </td>
            </tr>
          </th>
        </thead>
      </table>
    </div>
  </div>

  <div class="">
    <div>
      <table v-if="selectedUser"
        class="">
        <thead class="">
          <th scope="col" class="">
            <tr>
              <th for="nama"> Nama: </th>
              <td><input
                  class=""
                  type="text" id="nama" v-model="selectedUser.nama" /></td>
            </tr>
          </th>
          <th scope="col" class="">
            <tr>
              <th for="kasus"> kasus: </th>
              <td><input
                  class=""
                  type="kasus" id="kasus" v-model="selectedUser.kasus" /></td>
            </tr>
          </th>
          <th scope="col" class="">
            <tr>
              <th for="jenis_kelamin"> jenis_kelamin: </th>
              <td><input
                  class=""
                  type="jenis_kelamin" id="jenis_kelamin" v-model="selectedUser.jenis_kelamin" /></td>
            </tr>
          </th>
          <th scope="col" class="">
            <tr>
              <td>
                <button
                  class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l"
                  type="submit" @click="updateData(selectedUser.id)">Update Data</button>
              </td>
              <td>
                
              </td>
            </tr>
          </th>
        </thead>
      </table>
    </div>
  </div>

</template>




