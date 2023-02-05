<template>
  <div class="input-filter">
    <input
      class="input-filter__content"
      value=""
      ref="ipFilter"
      :placeholder="placeholderInput"
      @change="updateValue()"
      @click.stop="showOption($event)"
      @keyup.enter="changeEnter($event)"
      @keyup.down="selectOptionDown()"
      @keyup.up="selectOptionUp()"
      @keyup="searchFilter()"
    />
    <div class="icon-down" @click.stop="showOption($event)"></div>
    <div class="input-filter-items" v-if="isShowOption">
      <div
        class="input-filter-item"
        ref="inputFilteritem"
        v-for="(typeRecord, index) in searchTypeRecords"
        :key="index"
        :id="typeRecord.id"
        @click.stop="changeOption(typeRecord.name, typeRecord.id)"
      >
        <b>{{ typeRecord.code }} -</b>   {{ typeRecord.name }}
      </div>
    </div>
  </div>
</template>

<script>
import Resource from "@/js/resource.js";

export default {
  name: "inputFilterForm",
  components: {},
  props: {
    modeFilter: Number,
    valueFirst: String,
    value: String,
  },
  data() {
    return {
      ContentText: Resource.ContentText, //Table content gọi từ file resource.js
      Tooltip: Resource.Tooltip, //Tooltip gọi từ file resource.js
      Placeholder: Resource.Placeholder, //Placeholder gọi từ file resource.js
      Title: Resource.Title, //Tiêu đề gọi từ file resource.js
      isShowOption: false,
      isAcive: false,
      searchTypeRecords: [],
      FilterAll: [],
      placeholderInput: "",
      fillterName: "",
      parts: [{
      "departmentId": "768f8e64-7d10-20c9-967d-e8c757976496",
      "departmentCode": "PB006",
      "departmentName": "Phòng bảo vệ",
    },
    {
      "departmentId": "4e272fc4-7875-78d6-7d32-6a1673ffca7c",
      "departmentCode": "PB007",
      "departmentName": "Phòng giám đốc",
    },
    {
      "departmentId": "469b3ece-744a-45d5-957d-e8c757976496",
      "departmentCode": "PB002",
      "departmentName": "Phòng hành chính",
    },
    {
      "departmentId": "4577565a-7e3e-493a-74dd-867949feb8b5",
      "departmentCode": "PB001",
      "departmentName": "Phòng marketing",
    },
    {
      "departmentId": "17120d02-6ab5-3e43-18cb-66948daf6128",
      "departmentCode": "PB000",
      "departmentName": "Phòng nhân sự",
    },
    {
      "departmentId": "11452b0c-768e-5ff7-0d63-eeb1d8ed8cef",
      "departmentCode": "PB009",
      "departmentName": "Phòng thu mua",
    },
    {
      "departmentId": "142cb08f-7c31-21fa-8e90-67245e8b283e",
      "departmentCode": "PB005",
      "departmentName": "Phòng đào tạo",
    }], // Department gọi từ API
      typeAssets:[
        {
      "fixedAssetCategoryId": "647a538e-630d-39cd-0144-e5a214c15aa7",
      "fixedAssetCategoryCode": "LTS002",
      "fixedAssetCategoryName": "Bàn",
    },
    {
      "fixedAssetCategoryId": "7c4f14d8-66fb-14ae-198f-6354f958f4c0",
      "fixedAssetCategoryCode": "LTS008",
      "fixedAssetCategoryName": "Bình lọc nước",
    },
    {
      "fixedAssetCategoryId": "35e773ea-5d44-2dda-26a8-6d513e380bde",
      "fixedAssetCategoryCode": "LTS003",
      "fixedAssetCategoryName": "Ghế",
    },
    {
      "fixedAssetCategoryId": "26fe1532-7e63-6a11-1a8f-6354f958f4c0",
      "fixedAssetCategoryCode": "LTS001",
      "fixedAssetCategoryName": "Máy chiếu",
    },
    {
      "fixedAssetCategoryId": "3f8e6896-4c7d-15f5-a018-75d8bd200d7c",
      "fixedAssetCategoryCode": "LTS000",
      "fixedAssetCategoryName": "Máy in",
    },
    {
      "fixedAssetCategoryId": "78aafe4a-67a7-2076-3bf3-eb0223d0a4f7",
      "fixedAssetCategoryCode": "LTS004",
      "fixedAssetCategoryName": "Máy lạnh",
    },
    {
      "fixedAssetCategoryId": "7c48a057-3c21-3db0-06a4-60fcae00630c",
      "fixedAssetCategoryCode": "LTS005",
      "fixedAssetCategoryName": "Máy tính",
    },
    {
      "fixedAssetCategoryId": "6f45e010-3200-5943-27a8-6d513e380bde",
      "fixedAssetCategoryCode": "LTS006",
      "fixedAssetCategoryName": "Tai nghe",
    },
    {
      "fixedAssetCategoryId": "45ac3d26-18f2-18a9-3031-644313fbb055",
      "fixedAssetCategoryCode": "LTS007",
      "fixedAssetCategoryName": "Tủ",
    },
    {
      "fixedAssetCategoryId": "75cce5a6-4f7f-3671-3cf3-eb0223d0a4f7",
      "fixedAssetCategoryCode": "LTS009",
      "fixedAssetCategoryName": "Đèn",
    }
      ], // Loại tài sản gọi từ API
      FirstName: ""
    };
  },
  created() {
    this.fillterName = this.valueFirst;

    let _this = this;

    // Sự kiện khi click ra ngoài thì đóng thẻ select
    window.addEventListener("click", function () {
      try {
        _this.isShowOption = false;
      } catch (error) {
        console.log(error);
      }
    });

    switch (this.modeFilter) {
      case 1:
        // Thay đổi placehoder
        this.placeholderInput = this.Placeholder.CategoryAsset;

        // load danh sách loại tài sản
        try {
          fetch("http://localhost:63515/api/AssetCategorys/filter?keyword=")
            .then((response) => {
              return response.json();
            })
            .then((AssetCategory) => {
                this.searchTypeRecords = AssetCategory.data.map(function(assetCategory){
                  return {
                    id: assetCategory.fixedAssetCategoryId,
                    code: assetCategory.fixedAssetCategoryCode,
                    name: assetCategory.fixedAssetCategoryName
                  }
                });
                this.FilterAll = this.searchTypeRecords;
            });
        
        } catch (error) {
          console.log(error);
        }

        // Thay đổi value ban đầu(nếu có)
        if(this.valueFirst){
          for (const typeAsset of this.typeAssets) {
            if(typeAsset.fixedAssetCategoryId == this.valueFirst){
              this.FirstName = typeAsset.fixedAssetCategoryName;
            }
          }
        }

        break;

      case 2:
        // Thay đổi placehoder
        this.placeholderInput = this.Placeholder.Department;

        // load danh sách loại bộ phạn sử dụng
        try {
          fetch("http://localhost:63515/api/v1/Departments/filter?keyword=")
            .then((response) => {
              return response.json();
            })
            .then((Departments) => {
                this.searchTypeRecords = Departments.data.map(function(department){
                  return {
                    id: department.departmentId,
                    code: department.departmentCode,
                    name: department.departmentName
                  }
                });
                this.FilterAll = this.searchTypeRecords;
            });
        } catch (error) {
          console.log(error);
        }
        
        // Thay đổi value ban đầu(nếu có)
        if(this.valueFirst){
          for (const part of this.parts) {
            if(part.departmentId == this.valueFirst){
              this.FirstName = part.departmentName;
            }
          }
        }

        break;
      default:

        break;
    }
  },
  mounted(){
    var _this = this;

    switch (this.modeFilter) {
      case 1:
        // Thay đổi value ban đầu(nếu có)
        if(this.valueFirst){
          for (const typeAsset of this.typeAssets) {
            if(typeAsset.fixedAssetCategoryId == this.valueFirst){
              _this.$refs.ipFilter.value = typeAsset.fixedAssetCategoryName;
            }
          }
        }
        break;
    
      case 2:
        // Thay đổi value ban đầu(nếu có)
        if(this.valueFirst){
          for (const part of this.parts) {
            if(part.departmentId == this.valueFirst){
              _this.$refs.ipFilter.value = part.departmentName;
            }
          }
        }
        break;
      default:
        break;
    }
  },
  methods: {

    // Hiện các option khi click vào filter
    showOption(e) {
      e.stopImmediatePropagation();
      this.isShowOption = true;
    },

    changeEnter(e){
      let _this = this;

      if(this.isShowOption == false){
        e.stopImmediatePropagation();
        this.isShowOption = true;
      }else{
        let ipFilters = this.$refs.inputFilteritem;

        for (const ipFilter of ipFilters) {
          // kiểm tra xem thẻ option đó không phải thẻ dưới cùng và là thẻ đang đc active mới thực hiện
          if(ipFilter.classList.contains('active')){
            for(let searchTypeRecord of _this.FilterAll) {
              if(ipFilter.id == searchTypeRecord.id){
                _this.changeOption(searchTypeRecord.name, searchTypeRecord.id)
              }
            }
            break;
          }
        }
        _this.isAcive = false;
      }
    },

    // Chọn option bằng mũi tên xuống
    selectOptionDown(){
      let ipFilters = this.$refs.inputFilteritem;

      if(this.isAcive){
        for (const ipFilter of ipFilters) {
          // kiểm tra xem thẻ option đó không phải thẻ dưới cùng và là thẻ đang đc active mới thực hiện
          if(ipFilter.nextElementSibling != null && ipFilter.classList.contains('active')){
            ipFilter.classList.remove("active");
            ipFilter.nextElementSibling.classList.add("active");
            break;
          }
        }
      }else{
        ipFilters[0].classList.add("active")
        this.isAcive = true;
      }
    },

    // Chọn option bằng mũi tên lên
    selectOptionUp(){
      let ipFilters = this.$refs.inputFilteritem;

      if(this.isAcive){
        for (const ipFilter of ipFilters) {
          // kiểm tra xem thẻ option đó không phải thẻ trên cùng và là thẻ đang đc active mới thực hiện
          if(ipFilter.previousElementSibling != null && ipFilter.classList.contains('active')){
            ipFilter.classList.remove("active");
            ipFilter.previousElementSibling.classList.add("active");
            break;
          }
        }
      }else{
        ipFilters[ipFilters.length - 1].classList.add("active")
        this.isAcive = true;
      }
    },

    //Ẩn option khi chọn xong option
    hideOption() {
        this.isShowOption = false;
    },

    // Thay đổi option và ẩn nó đi khi chọn xong
    changeOption(Name, Id) {
      let _input = this.$refs.ipFilter;

      _input.value = Name;

      // Thực hiện lọc
      this.$emit("filter", Id);

      // Đóng option
      this.isShowOption = false;
    },

    updateValue(){
    },

    // 
    searchFilter(){
      let _input = this.$refs.ipFilter;

      this.searchTypeRecords = this.FilterAll.filter(function(Record){
        return Record.name.includes(_input.value);
      });
    },

    // // 
    // getData : async() => {
    //   var _this = this;
    //   // load danh sách phòng ban
    //   try {
    //     // axios({
    //       //   method: 'get',
    //       //   url: 'http://localhost:63515/api/v1/Departments/filter?keyword=',
    //       //   responseType: 'stream'
    //       // })
    //       //   .then(function (response) {
    //       //     console.log(response.data);
    //       //   });
    //     axios.get("http://localhost:63515/api/v1/Departments/filter?keyword=")
    //       .then((response) => {
    //         _this.parts = response.data.data;
    //         console.log(_this.parts);
    //       });
    //   } catch (error) {
    //     console.log(error);
    //   }
    // }
  },
};
</script>

<style scoped>
.input-filter {
  background-color: #fff;
  width: 100%;
  height: 34px;
  border-radius: 4px;
  display: flex;
  position: relative;
}

select {
  border: none;
  outline: none;
  width: 174px;
  font-weight: 500;
}

.input-filter__content {
  flex: 1;
  line-height: 34px;
  font-size: 14px;
  flex: 1;
  padding-left: 10px;
  cursor: context-menu;
  border: none;
  outline: none;
  cursor: auto;
}

.input-filter .icon-down {
  background: url("../../../assets/icon/qlts-icon.png") no-repeat -62px -330px;
  width: 20px;
  height: 20px;
  margin: 7px;
}

.input-filter-items {
  position: absolute;
  margin-top: 2px;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  z-index: 1;
  width: 100%;
  top: 35px;
  max-height: 180px;
  overflow-y: scroll;
  overflow-x: hidden;
}

.input-filter-item {
  display: flex;
  background-color: #ffffff;
  width: 100%;
  height: 34px;
  font-size: 14px;
  line-height: 30px;
  padding-left: 10px;
}

b{
    margin-right: 3px;
}

.input-filter-item:hover {
  background-color: rgb(176, 219, 229);
  cursor: pointer;
}

.active{
  background-color: rgb(176, 219, 229);
}

.input-filter-item:focus {
  background-color: #000;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  background: #f4f7ff;
}

::-webkit-scrollbar-thumb {
  background: #ccc;
}
</style>