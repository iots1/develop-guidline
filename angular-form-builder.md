# การใช้ Form Builder

Inject FormBuilder
``` typescript
  /* ใน Module ที่จะใช้ FormBuilder ต้อง import ReactiveFormsModule ก่อนใช้งาน */
  constructor(
    private _formBuilder: FormBuilder
  ) { }
```

สร้าง FormGroup
``` typescript
  form: FormGroup
```

สร้าง Method สำหรับ Initial Form
``` typescript
  this.form = this._formBuilder.group({
    _id: [],
    customer_avatar: [],
    customer_first_name: ['', Validators.required],
    customer_last_name: ['', Validators.required],
    customer_phone_number: [],
    customer_email: ['', Validators.email],
    customer_address_no: [],
    customer_address_moo: [],
    customer_address_road: [],
    customer_address_sub_district: [],
    customer_address_district: [],
    customer_address_province: [],
    customer_address_postal_code: [],
    customer_address: [],
    customer_remark: [],
  })
```
สร้าง Method OnInitForm แล้วเรียกใช้งาน
``` typescript
  ngOnInit(): void {
    this.onInitForm();
  }
  
  onInitForm(): void {
     this.form = this._formBuilder.group({
      _id: [],
      customer_avatar: [],
      customer_first_name: ['', Validators.required],
      customer_last_name: ['', Validators.required],
      customer_phone_number: [],
      customer_email: ['', Validators.email],
      customer_address_no: [],
      customer_address_moo: [],
      customer_address_road: [],
      customer_address_sub_district: [],
      customer_address_district: [],
      customer_address_province: [],
      customer_address_postal_code: [],
      customer_address: [],
      customer_remark: [],
    })
  }
  
  /* ใช้ submit เพื่อบันทึกหรือแก้ไข form */
  onSubmit(): void {
     if (this.form.invalid) return this.form.markAllAsTouched();
     /* call api save or edit */
  }
```

Ex. Template for component
``` html
<form (submit)="onSubmit()"> 
  <input formControlName="customer_first_name" />
  <input formControlName="customer_last_name" />
  ...
  
  <button type="submit">Save</button>
</form>
```
