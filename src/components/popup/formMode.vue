<template>
  <div class="black-ground bg-add">
    <div class="form-add">
      <div class="form-add__container">
        <div class="form-add__container__head">
          <div class="form-add__container__head__title">{{ TheTitle }}</div>
          <div
            @click="hidenForm"
            class="icon-close"
            :title="Tooltip.Close"
          ></div>
        </div>
        <div class="form-add__container__body">
          <form class="form" id="form-1">
            <div class="form-row">
              <div class="form-group col1-3">
                <label for="fname"
                  >{{ ContentText.FixedAssetCode }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <input
                  @blur="blurValidate($event)"
                  @input="inputValide($event)"
                  id="AssetCode"
                  class="form-control"
                  type="text"
                  name="fname"
                  v-model="FixedAsset.fixedAssetCode"
                  :placeholder="Placeholder.ImportAssetCode"
                  maxlength="100"
                  autocomplete="off"
                />
                <div class="form-message"></div>
              </div>
              <div class="form-group col2-3">
                <label for="fname"
                  >{{ ContentText.FixedAssetName }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <input
                  @blur="blurValidate($event)"
                  @input="inputValide($event)"
                  id="AssetName"
                  class="form-control"
                  type="text"
                  name="fname"
                  v-model="FixedAsset.fixedAssetName"
                  :placeholder="Placeholder.ImportAssetName"
                  maxlength="255"
                  autocomplete="off"
                />
                <div class="form-message"></div>
              </div>
            </div>
            <div class="form-row">
              <div class="form-group col1-3">
                <label for="fname"
                  >{{ ContentText.DepartmentCode }}
                  <span class="cl-red">{{ ContentText.red }}</span></label
                >
                <div class="col-select">
                  <input
                    @click="showOption($event)"
                    id="PartCode"
                    class="option-filter__content"
                    :placeholder="Placeholder.ImportDepartmentCode"
                    :value="getPartCodeByDepartmentId(FixedAsset.departmentId)"
                    readonly
                  />
                  <div
                    class="icon-down select-option"
                    @click="showOption($event)"
                  ></div>
                  <div class="option-filter-items">
                    <div
                      class="option-filter-item"
                      ref="optionFilteritem"
                      v-for="(department, index) in Departments"
                      :key="index"
                      @click="
                        changeOptionPart(
                          $event,
                          department.departmentName,
                          department.departmentId
                        )
                      "
                    >
                      <b>{{department.departmentCode}}</b> - {{ department.departmentName }}
                    </div>
                  </div>
                </div>
              </div>
              <div class="form-group col2-3">
                <label for="fname">{{ ContentText.DepartmentName }} </label>
                <input
                  id="PartName"
                  class="form-control bg-ccc"
                  type="text"
                  name="fname"
                  :value="getPartNameByDepartmentId(FixedAsset.departmentId)"
                  placeholder=""
                  readonly
                  autocomplete="off"
                />
                <div class="form-message"></div>
              </div>
            </div>
            <div class="form-row">
              <div class="form-group col1-3">
                <label for="fname"
                  >{{ ContentText.FixedAssetCategoryCode }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <div class="col-select">
                  <input
                    @click="showOption($event)"
                    class="option-filter__content"
                    :placeholder="Placeholder.ImportCategoryAssetCode"
                    :value="
                      getTypeAssetCodeByFixedAssetCategoryId(
                        FixedAsset.fixedAssetCategoryId
                      )
                    "
                    readonly
                  />
                  <div
                    class="icon-down select-option"
                    @click="showOption($event)"
                  ></div>
                  <div class="option-filter-items">
                    <div
                      class="option-filter-item"
                      ref="optionFilteritem"
                      v-for="(assetCategory, index) in AssetCategorys"
                      :key="index"
                      @click="
                        changeOptionTypeAsset(
                          $event,
                          assetCategory.fixedAssetCategoryName,
                          assetCategory.fixedAssetCategoryId
                        )
                      "
                    >
                      <b>{{assetCategory.fixedAssetCategoryCode}}</b> - {{ assetCategory.fixedAssetCategoryName }}
                    </div>
                  </div>
                </div>
              </div>
              <div class="form-group col2-3">
                <label for="fname">{{
                  ContentText.FixedAssetCategoryName
                }}</label>
                <input
                  id="TypeAssetName"
                  class="form-control bg-ccc"
                  type="text"
                  name="fname"
                  :value="
                    getTypeAssetNameByFixedAssetCategoryId(
                      FixedAsset.fixedAssetCategoryId
                    )
                  "
                  readonly
                  placeholder=""
                  autocomplete="off"
                />
                <div class="form-message"></div>
              </div>
            </div>
            <div class="form-row">
              <div class="form-group col1-3">
                <label for="fname"
                  >{{ ContentText.Quantity }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <div class="out-ip">
                  <input
                    class="form-control text-align-right padding-right-20"
                    type="text"
                    name="fname"
                    v-model="FixedAsset.quantity"
                    maxlength="5"
                    autocomplete="off"
                  />
                  <div class="icon-UpAndDown">
                    <div class="icon-up"></div>
                    <div class="icon-down"></div>
                  </div>
                </div>
                <div class="form-message"></div>
              </div>
              <div class="form-group col1-3 form-group col1-3-center">
                <label for="fname"
                  >{{ ContentText.Cost }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <input
                  class="form-control text-align-right padding-right-20"
                  ref="cost"
                  type="text"
                  name="fname"
                  @keyup="importCost"
                  value="0"
                  maxlength="25"
                  autocomplete="off"
                />
              </div>
              <div class="form-group col1-3">
                <label for="fname"
                  >{{ ContentText.LifeTime }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <input
                  class="form-control text-align-right padding-right-20"
                  type="text"
                  name="fname"
                  v-model="FixedAsset.lifeTime"
                  maxlength="3"
                  autocomplete="off"
                />
              </div>
            </div>
            <div class="form-row">
              <div class="form-group col1-3">
                <label for="fname"
                  >{{ ContentText.DepreciationRate
                  }}<span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <input
                  class="form-control text-align-right padding-right-20"
                  type="text"
                  name="fname"
                  v-model="FixedAsset.depreciationRate"
                  maxlength="6"
                  autocomplete="off"
                />
                <div class="form-message"></div>
              </div>
              <div class="form-group col1-3 form-group col1-3-center">
                <label for="fname"
                  >{{ ContentText.ValueAtrophyYear }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <input
                  class="form-control text-align-right padding-right-20"
                  type="text"
                  ref="valueAtrophyYear"
                  name="fname"
                  @keyup="importValueAtrophyYear"
                  :value="0"
                  maxlength="25"
                  autocomplete="off"
                />
                <!-- @input="handleInputValueAtrophyYear" -->
              </div>
              <div class="form-group col1-3">
                <label for="fname">{{ ContentText.TrackedYear }} </label>
                <input
                  class="form-control bg-ccc text-align-right padding-right-20"
                  type="text"
                  name="fname"
                  readonly
                  v-model="FixedAsset.trackedYear"
                  autocomplete="off"
                />
              </div>
            </div>
            <div class="form-row">
              <div class="form-group col1-3 position-relative">
                <label for="fname"
                  >{{ ContentText.BuyDate }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <Datepicker
                  v-model="FixedAsset.purchaseDate"
                  placeholder="dd/mm/yyyy"
                  class="date-picker"
                  hideInputIcon
                  selectText="Chọn"
                  cancelText="Đóng"
                  format="dd/MM/yyyy"
                  :clearable="false"
                  :enableTimePicker="false"
                ></Datepicker>
                <div class="icon-datepicker"></div>
              </div>
              <div
                class="
                  form-group
                  col1-3
                  form-group
                  col1-3-center
                  position-relative
                "
              >
                <label for="fname"
                  >{{ ContentText.UseStartDate }}
                  <span class="cl-red">{{ ContentText.red }}</span>
                </label>
                <Datepicker
                  v-model="FixedAsset.useDate"
                  class="date-picker"
                  selectText="Chọn"
                  cancelText="Đóng"
                  format="dd/MM/yyyy"
                  :clearable="false"
                  :enableTimePicker="false"
                  hideInputIcon
                  ref="useDatePicker"
                ></Datepicker>
                <!--  -->
                <div
                  style="right: -268px"
                  class="icon-datepicker"
                  @click="handleClickUserDatePicker"
                ></div>
              </div>
              <div class="form-group col1-3"></div>
            </div>
          </form>
        </div>
        <div class="form-add__container__foot">
          <div @click="addData" type="sub" class="btn-submit">
            {{ ContentText.Save }}
          </div>
          <div @click="canceForm" type="submit" class="btn-cancel">
            {{ ContentText.Cancer }}
          </div>
        </div>
      </div>
    </div>
    <dialog-validate
      v-if="isShowDialogValidate"
      :validateItems="validateItems"
      :isShowDialogValidate="isShowDialogValidate"
    />
    <dialogCance v-if="isShowDialogCance" />
  </div>
</template>

<script>
import axios from "axios";
import Enum from "@/js/enum.js";
import Resource from "@/js/resource.js";
// import EnumDTO from "@/js/enumDTO.js";
import { FixedAsset } from "@/class/FixedAsset.js";
import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";
import dialogValidate from "@/components/dialog/dialogValidate.vue";
import dialogCance from "@/components/dialog/dialogCance.vue";

export default {
  name: "formMode",
  components: { Datepicker, dialogValidate, dialogCance },
  props: {
    // Chế độ của popup
    mode: Number,
    updateAsset: Object,
    duplicateAsset: Object,
  },
  data() {
    return {
      ContentText: Resource.ContentText, //Table content gọi từ file resource.js
      Title: Resource.Title, //Tiêu đề gọi từ file resource.js
      Placeholder: Resource.Placeholder, //Placeholder gọi từ file resource.js
      Tooltip: Resource.Tooltip, //Tooltip gọi từ file resource.js
      FixedAsset: new FixedAsset(), // Đối tượng tài sản sẽ nhận data từ form
      TheTitle: "", // Tiêu đề của form
      AssetCategorys: [], // Danh sách loại tìa sản lấy từ API
      Departments: [], // Danh sách bộ phận sử dụng lấy từ API
      isShowDialogValidate: false,
      isShowDialogCance: false,
      validateItemss: [], // Mảng các trường chưa nhập cần validate(cảnh báo)
      assetCodeAuto: "", // Mã tài sản tự động lấy được
      valueAtrophyYearBefor: "0", //Giá trị của giá trị hao mòn năm trước lúc nhập (trước khi bắt keyup)
      costBefor: "0", // Giá trị của nguyên giá trước lúc nhập (trước khi bắt keyup)
    };
  },
  created() {
    var _this = this;

    // Load dữ liệu trang
    this.loadData();

    // Sự kiện khi click ra ngoài thì đóng thẻ select
    window.addEventListener("click", function () {
      try {
        // Chon tất cả thẻ option
        let listSelectControl = _this.$refs.optionFilteritem;

        // Ẩn hết toàn bộ thẻ select đang mở
        for (const selectControl of Array.from(listSelectControl)) {
          selectControl.style.display = "none";
        }
      } catch (error) {
        console.log(error);
      }
    });

    try {
      // cập nhật đối tượng tường ứng với từng loại popup
      switch (this.mode) {
        case Enum.Mode.Update:
          // Thiết lập title popup thành Sửa tài sản
          this.TheTitle = this.ContentText.UpdateAsset;

          // Truyền giá trị tài sản ban đầu vào form
          this.CreateFormUpdate(this.updateAsset);

          break;
        case Enum.Mode.Duplicate:
          // Thiết lập title popup thành Sửa tài sản
          this.TheTitle = this.ContentText.AddAsset;

          // Truyền giá trị tài sản ban đầu vào form
          this.CreateFormDuplicate(this.duplicateAsset);

          // Lấy mã mới cho trường tài sản
          fetch("http://localhost:63515/api/v1/Assets/new-code")
            .then((response) => {
              return response.json();
            })
            .then((AssetCodeAuto) => {
              this.assetCodeAuto = AssetCodeAuto.newAssetCode;
              this.FixedAsset.fixedAssetCode = this.assetCodeAuto;
            });
          
          break;
        default:
          // Thiết lập title popup thành Thêm tài sản
          this.TheTitle = this.ContentText.AddAsset;

          // Tự sinh mã mới cho trường mã tài sản
          fetch("http://localhost:63515/api/v1/Assets/new-code")
            .then((response) => {
              return response.json();
            })
            .then((AssetCodeAuto) => {
              this.assetCodeAuto = AssetCodeAuto.newRecordCode;
              this.FixedAsset.fixedAssetCode = this.assetCodeAuto;
            });

          break;
      }
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    handleClickUserDatePicker() {
      let datepicker = this.$refs.useDatePicker;
      datepicker.openMenu();
    },
    // Hàm cảu input Linh
    // handleInputValueAtrophyYear(e) {
    //   e.target.value = e.target.value.toString()
    //     .replace(/\D/g, "")
    //     .replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    // },

    /**
     * Hàm Load data
     * Tạo bởi: NVAn(20/12/2022)
     */
    loadData() {
      // load danh sách tài sản
      this.isShowDialogLoading = true;

      // load danh sách loại tài sản
      try {
        fetch("http://localhost:63515/api/AssetCategorys/filter?keyword=")
          .then((response) => {
            return response.json();
          })
          .then((AssetCategory) => {
            this.AssetCategorys = AssetCategory.data;
          });
      } catch (error) {
        console.log(error);
      }

      // load danh sách phòng ban
      try {
        fetch("http://localhost:63515/api/v1/Departments/filter?keyword=")
          .then((response) => {
            return response.json();
          })
          .then((part) => {
            this.Departments = part.data;
          });
      } catch (error) {
        console.log(error);
      }
    },

    /**
     * Hàm format tiền str: String
     * Trả về string
     * Tạo bởi: NVAn(20/12/2022)
     */
    formatCash(str) {
      return str
        .split("")
        .reverse()
        .reduce((prev, next, index) => {
          return (index % 3 ? next : next + ".") + prev;
        })
    },

    /**
     * Hàm format tiền str: (String) (Xóa dấu phẩy)
     * Trả về number
     * Tạo bởi: NVAn(20/12/2022)
     */
    formatCashReverse(str) {
      return Number(str.split(".").join(""));
    },

    //Xử lý sự kiện khi blur vào ô input (ẩn hiện message validate)
    blurValidate(e) {
      let errorElement = e.target.parentElement.querySelector(".form-message");
      let inputElement = e.target.parentElement.querySelector("input");
      let errorMessage = e.target.parentElement.querySelector("input").value.trim() 
        ? undefined
        : "Vui lòng nhập trường này";
      if (errorMessage) {
        errorElement.innerText = errorMessage;
        inputElement.parentElement.classList.add("invalid");
      } else {
        errorElement.innerText = "";
        inputElement.parentElement.classList.remove("invalid");
      }
    },

    //Xử lý sự kiện khi nhập dữ liệu vào ô input (ẩn message validate)
    inputValide(e) {
      let errorElement = e.target.parentElement.querySelector(".form-message");
      let inputElement = e.target.parentElement.querySelector("input");
      errorElement.innerText = "";
      inputElement.parentElement.classList.remove("invalid");
    },

    //Sự kiện tạo thẻ select(blur và ra sẽ hiện option)
    showOption(e) {
      try {
        e.stopImmediatePropagation();
        let selectControls = e.target.parentElement.querySelectorAll(".option-filter-item");
        for (const selectControl of selectControls) {
          selectControl.style.display = "flex";
        }
      } catch (error) {
        console.log(error);
      }
    },

    /**
     * Hàm xử lý sự kiện khi thay đổi option thẻ select bộ phận sử dụng
     * Tạo bởi: NVAn(20/12/2022)
     */
    changeOptionPart(e, departmentName, departmentId) {
      // Thay đổi value ô input select
      e.target.parentElement.parentElement.querySelector(".option-filter__content").value = departmentName;

      // Chọn tên bộ phận tương ứng với mã bộ phận
      for (const department of this.Departments) {
        if (department.departmentId == departmentId) {
          this.FixedAsset.departmentId = departmentId;
        }
      }
    },

    /**
     * Hàm tự động tính giá trị hao mòn năm
     * Tạo bởi: NVAn(20/12/2022)
     */
    autoCalculateValueAtrophyYear() {
      // Lấy thẻ input của giá trị hao mòn năm
      let valueAtrophyYear = this.$refs.valueAtrophyYear;

      // Tự động tính giá trị hao mòn năm
      this.FixedAsset.valueAtrophyYear = Math.round(
        (this.FixedAsset.depreciationRate * this.FixedAsset.cost) / 100
      );

      //format tiền cho giá trị hao mòn năm và cập nhật
      valueAtrophyYear.value = this.formatCash(
        this.FixedAsset.valueAtrophyYear.toString()
      );

      // Cập nhật giá trị hao mòn năm cũ
      this.valueAtrophyYearBefor = valueAtrophyYear.value;
    },

    /**
     * Hàm bắt sự kiện keyup cho nguyên giá
     * Tạo bởi: NVAn(20/12/2022)
     */
    importCost(e) {
      let cost = this.$refs.cost;

      //Chỉ cho nhập số
      if ((e.keyCode >= 48 && e.keyCode <= 57) || e.keyCode === 8) {
        //format tiền cho nguyên giá
        cost.value = this.formatCash(
          this.formatCashReverse(cost.value).toString()
        );

        //gán giá trị vào cost
        this.FixedAsset.cost = this.formatCashReverse(cost.value);

        //Tự động tính giá trị hao mòn năm
        this.autoCalculateValueAtrophyYear();

        // Cập nhật nguyên giá cũ
        this.costBefor = cost.value;
      } else {
        //Nếu nhập không phải số

        // gán value cho giá trị value cũ
        cost.value = this.formatCash(
          this.formatCashReverse(this.costBefor).toString()
        );
      }

      // console.log("costBefor", this.costBefor);
      // console.log("FixedAsset.cost", this.FixedAsset.cost);
      // console.log("cost.value", cost.value);
    },

    /**
     * Hàm bắt sự kiên keyup cho giá trị hao mòn năm
     * Tạo bởi: NVAn(20/12/2022)
     */
    importValueAtrophyYear(e) {
      // Lấy thẻ input của giá trị hao mòn năm
      let valueAtrophyYear = this.$refs.valueAtrophyYear;

      //Chỉ cho nhập số
      if ((e.keyCode >= 48 && e.keyCode <= 57) || e.keyCode === 8) {
        // Nếu nhập số sẽ thực hiện:

        //format tiền cho giá trị hao mòn năm
        valueAtrophyYear.value = this.formatCash(
          this.formatCashReverse(valueAtrophyYear.value).toString()
        );

        //gán giá trị vào giá trị hao mòn năm
        this.FixedAsset.valueAtrophyYear = this.formatCashReverse(
          valueAtrophyYear.value
        );

        // Cập nhật giá trị hao mòn năm cũ
        this.valueAtrophyYearBefor = valueAtrophyYear.value;
      } else {
        //Nếu nhập không phải số

        // gán value cho giá trị value cũ
        valueAtrophyYear.value = this.formatCash(
          this.formatCashReverse(this.valueAtrophyYearBefor).toString()
        );
      }

      // console.log(this.valueAtrophyYearBefor);
      // console.log(valueAtrophyYear.value);
      // console.log(this.FixedAsset.valueAtrophyYear);
    },

    //Xử lý sự kiện khi thay đổi option thẻ select loại tài sản
    changeOptionTypeAsset(e, fixedAssetCategoryName, fixedAssetCategoryId) {
      // Thay đổi value ô input select
      e.target.parentElement.parentElement.querySelector(".option-filter__content").value =
        fixedAssetCategoryName;

      // Chọn tên loại tài sản tương ứng với mã loại tài sản
      for (const assetCategory of this.AssetCategorys) {
        if (assetCategory.fixedAssetCategoryId == fixedAssetCategoryId) {
          this.FixedAsset.fixedAssetCategoryId = fixedAssetCategoryId;
        }
      }

      //Chọn số năm sử dụng và tỷ lệ hao mòn theo loại tài sản đó
      for (const assetCategory of this.AssetCategorys) {
        if (assetCategory.fixedAssetCategoryId == fixedAssetCategoryId) {
          this.FixedAsset.lifeTime = assetCategory.lifeTime;
          this.FixedAsset.depreciationRate = assetCategory.depreciationRate;
          this.FixedAsset.trackedYear =
            this.FixedAsset.useDate.getYear() + 1900;
        }
      }
    },

    //Đóng form khi click close
    hidenForm() {
      try {
        this.$emit("hideForm");
      } catch (error) {
        console.log(error);
      }
    },
    //Click vào hủy
    canceForm() {
      try {
        this.$emit("hideForm");
      } catch (error) {
        console.log(error);
      }
    },

    // Thêm mới dữ liệu khi lưu
    addData() {
      try {
        // Khai báo các trường cần validate
        let checkValidate = false;
        checkValidate =
          this.FixedAsset.fixedAssetCode == false ||
          this.FixedAsset.fixedAssetName == false ||
          this.FixedAsset.departmentId == false ||
          this.FixedAsset.fixedAssetCategoryId == false ||
          this.FixedAsset.quantity == false ||
          this.FixedAsset.cost == false ||
          this.FixedAsset.lifeTime == false ||
          this.FixedAsset.depreciationRate == false ||
          this.FixedAsset.valueAtrophyYear == false ||
          this.FixedAsset.trackedYear == false;

        // Kiểm tra validate ở các trường
        if (checkValidate) {
          this.isShowDialogValidate = !this.isShowDialogValidate;
          this.validateItems = [];

          if (this.FixedAsset.fixedAssetCode === "")
            this.validateItems.push("Mã tài sản");
          if (this.FixedAsset.fixedAssetName === "")
            this.validateItems.push("Tên tài sản");
          if (this.FixedAsset.departmentId === "")
            this.validateItems.push("Mã bộ phận sử dụng");
          if (this.FixedAsset.fixedAssetCategoryId === "")
            this.validateItems.push("Mã loại tài sản");
          if (this.FixedAsset.quantity === 0)
            this.validateItems.push("Số lượng");
          if (this.FixedAsset.cost === 0) this.validateItems.push("Nguyên giá");
          if (this.FixedAsset.valueAtrophyYear === 0)
            this.validateItems.push("Giá trị hao mòn năm");
          if (this.FixedAsset.lifeTime === 0)
            this.validateItems.push("Số năm sử dụng");
          if (this.FixedAsset.depreciationRate === 0)
            this.validateItems.push("Tỷ lệ hao mòn");
        } else {
          //Kiểm tra xem thuộc loại form mode nào
          switch (this.mode) {
            case Enum.Mode.Update:
              axios
                .put(
                  `http://localhost:63515/api/v1/Assets/${this.FixedAsset.fixedAssetId}`,
                  {
                    fixedAssetId: this.FixedAsset.fixedAssetId,
                    fixedAssetCode: this.FixedAsset.fixedAssetCode,
                    fixedAssetName: this.FixedAsset.fixedAssetName,
                    departmentId: this.FixedAsset.departmentId,
                    fixedAssetCategoryId: this.FixedAsset.fixedAssetCategoryId,
                    purchaseDate: this.FixedAsset.purchaseDate,
                    cost: this.FixedAsset.cost,
                    quantity: this.FixedAsset.quantity,
                    depreciationRate: this.FixedAsset.depreciationRate,
                    trackedYear: this.FixedAsset.trackedYear,
                    lifeTime: this.lifeTime,
                    productionYear: this.productionYear,
                    valueAtrophyYear: this.valueAtrophyYear,
                    createdBy: "string",
                    createdDate: "2022-12-26T17:50:33.580Z",
                    modifiedBy: "string",
                    modifiedDate: "2022-12-26T17:50:33.580Z",
                  }
                )
                .then((res) => {
                  console.log(res);
                  this.$emit("onShowToastSuccess");
                });
              this.canceForm();
              break;

            // Mặc định sẽ là thêm mới
            default:
              axios
                .post("http://localhost:63515/api/v1/Assets", {
                  fixedAssetId: "3fa85f64-5717-4562-b3fc-2c963f66afa6",
                  fixedAssetCode: this.FixedAsset.fixedAssetCode,
                  fixedAssetName: this.FixedAsset.fixedAssetName,
                  departmentId: this.FixedAsset.departmentId,
                  fixedAssetCategoryId: this.FixedAsset.fixedAssetCategoryId,
                  purchaseDate: this.FixedAsset.purchaseDate,
                  cost: this.FixedAsset.cost,
                  quantity: this.FixedAsset.quantity,
                  depreciationRate: this.FixedAsset.depreciationRate,
                  trackedYear: this.FixedAsset.trackedYear,
                  lifeTime: this.lifeTime,
                  productionYear: this.productionYear,
                  valueAtrophyYear: this.valueAtrophyYear,
                  createdBy: "string",
                  createdDate: "2022-12-26T17:50:33.580Z",
                  modifiedBy: "string",
                  modifiedDate: "2022-12-26T17:50:33.580Z",
                })
                .then((res) => {
                  console.log(res);
                });
              console.log(this.FixedAsset);
              this.$emit("onShowToastSuccess");
              this.canceForm();
              break;
          }
        }
      } catch (error) {
        console.log(error);
      }
    },

    //Lấy tên bộ phận sử dụng bởi id bộ phận sử dụng
    getPartNameByDepartmentId(departmentId) {
      for (const department of this.Departments) {
        if (department.departmentId == departmentId)
          return department.departmentName;
      }
    },

    //Lấy mã bộ phận sử dụng bởi id bộ phận sử dụng
    getPartCodeByDepartmentId(departmentId) {
      for (const department of this.Departments) {
        if (department.departmentId == departmentId)
          return department.departmentCode;
      }
    },

    //Lấy tên loại tài sản bởi id loại tài sản
    getTypeAssetNameByFixedAssetCategoryId(fixedAssetCategoryId) {
      for (const assetCategory of this.AssetCategorys) {
        if (assetCategory.fixedAssetCategoryId == fixedAssetCategoryId)
          return assetCategory.fixedAssetCategoryName;
      }
    },

    //Lấy mã loại tài sản bởi id loại tài sản
    getTypeAssetCodeByFixedAssetCategoryId(fixedAssetCategoryId) {
      for (const assetCategory of this.AssetCategorys) {
        if (assetCategory.fixedAssetCategoryId == fixedAssetCategoryId)
          return assetCategory.fixedAssetCategoryCode;
      }
    },

    //Lấy số năm sử dụng bở mã loại tài sản
    getNumberYearOfUseByTypeAssetCode(TypeAssetCode) {
      for (const typeAsset of this.AssetCategorys) {
        if (typeAsset.TypeAssetCode == TypeAssetCode)
          return typeAsset.NumberYearOfUse;
      }
    },

    //Lấy tỷ lệ hao mòn dựa trên mã loại tài sản
    getWearRateByTypeAssetCode(TypeAssetCode) {
      for (const typeAsset of this.AssetCategorys) {
        if (typeAsset.TypeAssetCode == TypeAssetCode) return typeAsset.WearRate;
      }
    },

    //Truyền giá trị tài sản ban đầu vào form
    CreateFormUpdate(updateAsset) {
      this.FixedAsset = updateAsset;
    },

    //Truyền giá trị tài sản ban đầu vào form
    CreateFormDuplicate(duplicateAsset) {
      this.FixedAsset = duplicateAsset;
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,400;1,300;1,400;1,500&display=swap");
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.position-relative {
  position: relative;
}

.cl-red {
  color: red;
  position: relative;
  top: 3px;
}

.bg-ccc {
  background-color: rgb(225, 225, 225);
}

.black-ground {
  display: inline-block;
  box-sizing: border-box;
  position: fixed;
  background-color: rgba(0, 0, 0, 0.5);
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  z-index: 10;
}

.form-add {
  box-sizing: border-box;
  width: 1000px;
  height: 622px;
  top: calc((100vh - 622px) / 2);
  bottom: calc((100vh - 622px) / 2);
  right: calc((100vw - 1000px) / 2);
  left: calc((100vw - 1000px) / 2);
  background-color: #ffffff;
  position: fixed;
  z-index: 100;
  border-radius: 6px;
  overflow: hidden;
}

.form-add__container {
  width: 100%;
  height: calc(100% - 108px);
  box-sizing: border-box;
  margin-top: 24px;
}

.form-add__container__head {
  display: flex;
  justify-content: space-between;
  margin-left: 24px;
  margin-right: 24px;
}

.form-add__container__head__title {
  font-size: 20px;
  font-weight: 600;
}

.form-add__container__head .icon-close {
  background: url("../../assets/icon/qlts-icon.png") no-repeat -375px -287px;
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.form-add__container__body {
  margin-top: 24px;
  margin-left: 24px;
  margin-right: 24px;
  margin-bottom: 13px;
}

.form-row {
  display: flex;
  justify-content: space-between;
}

.col1-3 {
  display: flex;
  flex-direction: column;
  width: calc(100% / 3 - 12px);
}

.col2-3 {
  display: flex;
  flex-direction: column;
  width: calc(2 * 100% / 3 - 12px);
}

.col1-3-center {
  width: calc(100% / 3 - 23px) !important;
}

.form-row label {
  font-size: 14px;
  font-weight: 500;
}

.form-row .form-control {
  height: 36px;
  margin-top: 8px;
  padding-left: 8px;
  padding-right: 10px;
  font-size: 14px;
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  border: 1px solid #b3b3b3;
  border-radius: 3px;
  outline: none;
  width: 100%;
}

.form-control:hover {
  border-color: #1aa4c8;
}

.col-select {
  position: relative;
  margin-top: 8px;
  border: 1px solid #b3b3b3;
  font-size: 14px;
  font-style: italic;
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  border-radius: 3px;
  display: flex;
  width: 100%;
}

.col-select:hover {
  border-color: #1aa4c8;
}

.option-filter__content {
  padding-left: 8px;
  padding-right: 4px;
  flex: 1;
  height: 36px;
  border: none;
  outline: none;
  cursor: context-menu;
}

.select-option.icon-down {
  background: url("../../assets/icon/qlts-icon.png") no-repeat -62px -330px;
  width: 20px;
  height: 20px;
  margin: 7px;
}

.option-filter-items {
  position: absolute;
  margin-top: 3px;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  z-index: 10;
  width: 100%;
  left: -1px;
  top: 34px;
  max-height: 180px;
  overflow-y: scroll;
  overflow-x: hidden;
}

.option-filter-item {
  display: none;
  background-color: #ffffff;
  width: calc(100% + 2px);
  height: 36px;
  font-size: 14px;
  line-height: 34px;
  padding-left: 6px;
  font-style: normal;
}

.option-filter-item:hover {
  background-color: rgb(176, 219, 229);
  cursor: pointer;
}

.date-picker {
  margin-top: 8px;
}

.icon-datepicker {
  background: url("../../assets/icon/qlts-icon.png") no-repeat -287px -67px;
  width: 18px;
  height: 18px;
  position: relative;
  top: -26px;
  right: -276px;
}

.form-add__container__foot {
  width: 100%;
  height: 82px;
  background-color: rgb(227, 227, 227);
  display: flex;
  flex-direction: row-reverse;
}

.form-add__container__foot .btn-submit {
  width: 100px;
  height: 36px;
  margin: 12px 24px 12px 12px;
  line-height: 36px;
  text-align: center;
  background-color: #1aa4c8;
  border-radius: 2px;
  color: #ffffff;
  cursor: pointer;
}

.form-add__container__foot .btn-cancel {
  width: 100px;
  height: 36px;
  margin: 12px 0 12px 12px;
  line-height: 36px;
  text-align: center;
  background-color: #fff;
  border-radius: 2px;
  color: #000;
  cursor: pointer;
}

.form-group {
}

.form-group .form-message {
  font-size: 13px;
  height: 16px;
  line-height: 16px;
  color: #f33a58;
}

.form-group.invalid .form-control {
  border-color: #f33a58;
}

.text-align-right {
  text-align: right;
}

.padding-right-20 {
  padding-right: 20px !important;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
}

.out-ip {
  display: flex;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  background: #f4f7ff;
  border-radius: 5px;
}

::-webkit-scrollbar-thumb {
  background: #ccc;
}
</style>


