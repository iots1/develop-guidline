# รูปแบบประกาศตัวแปร
- ไฟล์ **.model.ts จะถูกตั้งชื่อ Class ให้ตรงกับ Table หรือ Collection ฐานข้อมูล ใช้ตัวแปรแบบ snack case
- Class ขึ้นต้นด้วยตัวใหญ่ เช่น CustomerListComponent
- Method ขึ้นต้นด้วยตัวเล็ก เช่น onSubmit, onInitData, onInitForm
- ตัวแปร private ขึ้นต้นด้วย _ เช่น _customer, _alert, _modalService
- ตัวแปร public ใช้แบบ camel case
- ตัวแปร enum ใช้ Upper case & snack case

# รูปแบบตั้งชื่อ collection & field
- รูปแบบ collection_field-name
- กรณี Array collection_field-name_items (Array) หรือ collection_field-name พหุพจน์
```
users
  user_first_name
  user_last_name
  user_email
  user_ddresses (array)
```

# NestJS
#### Module
``` typescript
/* MeterModel, MeterSchema ใช้แบบพหุพจน์ ชื่อ Collections ใช้ เอกพจน์ */

@Module({
  imports: [
    MongooseModule.forFeature([
      { name: 'meters', schema: MeterSchema }
    ])
  ],
  controllers: [
    MetersController
  ],
  providers: [
    MetersService
  ]
})
export class MetersModule { }
```

#### Schema & Model
``` typescript
import { Timestamps } from '../../../shared/interfaces/timestamps.model';
import { ApiProperty } from '@nestjs/swagger';
import { IsNumber, IsOptional, IsString } from 'class-validator';
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';

@Schema({ timestamps: { createdAt: 'created_at', updatedAt: 'updated_at' } })
export class MeterModel extends Timestamps {

    @ApiProperty({ description: 'ประเภทมิเตอร์ ไฟฟ้า น้ำ' })
    @Prop({ type: Number, enum: [1, 2], default: 1 })
    @IsNumber()
    @IsOptional()
    meter_type: METER_TYPE

    @ApiProperty({ description: 'เลขมิเตอร์ก่อนหน้า' })
    @Prop()
    @IsNumber()
    meter_start_unit: number;

    @ApiProperty({ description: 'เลขมิเตอร์ล่าสุด' })
    @Prop()
    @IsNumber()
    meter_end_unit: number;

    @ApiProperty({ description: 'ตัวคูณ' })
    @Prop()
    @IsNumber()
    meter_unit_price: number;

    @ApiProperty({ description: 'id แผนผัง' })
    @Prop()
    @IsString()
    meter_layout_id: string;

    @ApiProperty({ description: 'ชื่อแผนผัง' })
    @Prop()
    @IsString()
    meter_layout_name: string;
}

export const MeterSchema = SchemaFactory.createForClass(MeterModel);

enum METER_TYPE {
    ELECTRIC = 1,
    WATER
}

```

#### Controller
``` typescript
@Controller('v1/meters')
@ApiTags('Meters')
@ApiBearerAuth()
export class MetersController {
    constructor(
        private _meters: MetersService,
    ) { }

    @Get()
    async findAll(@Req() req: any) {
        return await this._meters.findAll(req.query);
    }

    @Get(':meterId')
    async findOne(@Param('meterId') meterId: string) {
        return await this._meters.findOne(meterId);
    }

    @Post()
    async create(@Req() req: any, @Body() data: MeterModel) {
        data.created_by = req.user.user_id;
        return await this._meters.create(data);
    }

    @Put(':meterId')
    async update(@Param('meterId') meterId: string, @Body() data: MeterModel) {
        return await this._meters.update(meterId, data);
    }

    @Delete(':meterId')
    async delete(@Param('meterId') meterId: string) {
        return await this._meters.delete(meterId);
    }
}
```

#### Service
``` typescript
@Injectable()
export class MetersService extends MongoCRUD<MeterModel> {
    constructor(
        @InjectModel('meters') public model: Model<MeterModel>,
    ) {
        super(model);
    }
}

```


# Angular
โครงสร้าง Module อย่างน้อยต้องมี 3 โฟลเดอร์ หากมี pipe directive หรืออื่นๆ ใช้เฉพาะ Module นั้นสามารถประกาศหรือสร้างเพิ่มเติมได้ ถ้าใช้ร่วมกันให้ประกาศไว้ที่ SharedModule
```
*การตั้งชื่อ หน้าตาราง  component_name-list.ts
*การตั้งชื่อ Popup หรือ Dialog กรอกข้อมูล ตั้งชื่อ dialog-component_name-form.ts


├── customers
    ├── dialogs (Optional)
        ├── dialog-customer-form.ts
    ├── service
        ├── customers.service.ts
    ├── components (Optioncal)
    ├── pages   
        ├── customer-list.ts
    ├── customers-routing.module.ts
    ├── customers.module.ts
```
