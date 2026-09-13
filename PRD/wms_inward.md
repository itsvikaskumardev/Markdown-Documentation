

# Purchase Order:

**PO (Purchase Order)** ek official aur legal document hota hai jo ek **Buyer** (Aapki company/Warehouse) apne **Supplier/Vendor** ko bhejti hai. 
Is document me saaf-saaf likha hota hai ki:
* Hame kaun-kaun se items chahiye?
* Kitni quantity me chahiye?
* Kis rate (price) par fix baat hui hai?
* Aur kis date tak delivery warehouse par pahunch jani chahiye?

Jaise hi Vendor is PO ko accept karta hai, ye ek official contract ban jata hai. Aur yahi PO Number aage chalkar Gate Inward par check kiya jata hai (Jo humne first step me dekha tha).

---

### PO Se Pehle Ki Kahani (The Story Before PO)
Ek warehouse me kisi cheez ka PO banne se pehle mainly ye 4 steps hote hain:

**1. Demand Planning (Zaroorat Pata chalna)**
Sabse pehle system (Inventory Management System) ya team ko realize hota hai ki warehouse me kisi item ka stock kam ho raha hai (Reorder point hit ho gaya hai). Ya fir market me kisi naye item ki demand hai jise mangwana hai.

**2. Purchase Requisition - PR (Internal Request)**
Inventory team apne Purchasing (Khariddari) department ko ek internal request bhejti hai jise **PR (Purchase Requisition)** kehte hain. Ye ek internal approval hota hai ki *"Bhai, hamare warehouse me 1000 boxes Soap ki zaroorat hai, please mangwa lo."*

**3. Vendor Selection (Supplier Dhundhna aur Bargain karna)**
Purchasing department dekhta hai ki ye sabun kaunsa vendor sabse sasta aur jaldi dega. Agar unke paas pehle se fixed vendor hai, toh seedha usko pakadte hain. Agar nahi, toh alag-alag vendors se **Quotation (Bhav)** mangwaye jate hain aur ek best vendor ko select kiya jata hai.

**4. PO Generate aur Send karna (Order Finalize)**
Jab vendor aur price fix ho jate hain, tab Purchasing team officially ek **Purchase Order (PO)** generate karti hai (Jisme PO Number, Items, Quantity, Rate hota hai) aur usko Vendor ko email ya system ke through bhej deti hai.

**5. Vendor ki Taiyari (Dispatch)**
Vendor us PO ko dekhta hai, apne warehouse me us maal ko pack karta hai, gaadi me load karta hai, uske sath ek **Invoice (Bill)** lagata hai, aur gaadi ko aapke warehouse ke liye rawana (dispatch) kar deta hai.



---
# Warehouse Inward Flow 

---

### 1. Inward Flow kahan se start hota hai?
Inward Flow tab start hota hai jab koi supplier ya vendor apna maal (goods/inventory) truck ya kisi vehicle me le kar warehouse ke **Gate** par pohochta hai.

### 2. Iska first step/event kya hota hai?
Iska sabse pehla step/event **Gate Inward** ki entry hota hai. 
Jaise hi truck warehouse pahunchta hai, system me ek `CreateGateInwardCommand` run hota hai. Is step me vehicle ki details, driver ka naam, aur vendor documents (jaise Invoice ya Purchase Order) system me record kiye jate hain.

### 3. Inward Flow ke main steps kya hain?
Inward Flow me mainly ye 5 steps hote hain:

1. **Gate Inward (Entry):** Truck aur driver ki details capture karna aur entry dena.
2. **Dock Assignment:** Truck ko kisi specific **Dock** (unloading area) assign karna jahan se goods ko unload kiya ja sake.
3. **GRN (Goods Receipt Note):** Unloading ke baad items ki counting ki jati hai aur system me GRN banaya jata hai (`CreateGrnCommand`). Isme verify kiya jata hai ki Purchase Order ke hisaab se correct items aur correct quantity aayi hai ya nahi. Phir is GRN ko approve kiya jata hai.
4. **QC (Quality Check):** Received items ki quality check ki jati hai. Jo items damaged hote hain ya theek nahi hote, unko alag (Rejection/RTV) kar diya jata hai.
5. **Putaway:** Jo items QC me pass ho jate hain, unhe warehouse ke final specific racks/bins me physically rakhne ka process Putaway kahlata hai. Jab items ko unke designated bins me rakh kar system me confirm kar diya jata hai (`ConfirmPutawayCommand`), tab Inward flow finally complete hota hai.

### 4. Ye Outward Flow se kis step par linked hai?
Inward Flow aur Outward Flow aapas me **"Inventory" (Bin Locations)** ke step par linked hain. 

- Jab Inward flow ka last step yaani **Putaway complete** hota hai, tab system me un items ki **Available Quantity** (Inventory) update ho jati hai. 
- Jaise hi ye inventory "Available" status me aati hai, wahin se **Outward Flow isko consume karna shuru kar sakta hai.** 
- Matlab, jab koi naya **Order** aata hai, to system Inward se aayi hui is nayi available inventory ko allocate kar leta hai aur wahin se Outward process (jaise **PickList** banna aur **Picking** start hona) shuru ho jata hai. 

Short me: **Inward flow "Inventory banata hai" aur Outward flow usi "Inventory ko use karta hai".** Inka joining link system ka **Inventory Update** step hai.

------
------


# Step 1:  Gate Inward (Entry)

Jab koi truck warehouse ke gate par aata hai, toh security guard ya operator system me **"Create Gate Inward"** ki entry karta hai. 

### 1. Data Capture (Information Ikhatta karna)
Sabse pehle system me truck aur usme rakhe maal (goods) ki basic jankari daali jati hai:

* **PO Numbers (Purchase Orders):** Vendor ne kaunse Purchase Order ke against maal bheja hai, unke numbers system me dale jate hain.

* **Transport Details:** Gaadi ka number (**Vehicle Number**), driver ka naam (**Driver Name**), aur driver ka mobile number enter hota hai.

* **Photo / Image:** Security point par gaadi ya documents ki photo click karke upload ki jati hai.

* **Other Details:** Kis gate se entry hui (Gate Number), aur kya security check complete hua ya nahi.


### 2. Validations & Checks (System dwara Checking)
Entry hone ke baad, system kuch important validations karta hai aage badhne se pehle:

* **Purchase Order (PO) Check:** System database me check karta hai ki driver ne jo PO numbers bataye hain, kya sach me wo orders humne place kiye they? Agar invalid ya galat PO number hoga toh error aayega.

* **Duplicate Entry Check:** System cross-check karta hai ki kahin yeh same gaadi (Vehicle) ya driver pehle se hi warehouse ke andar (IN_YARD) toh nahi hai? Agar already andar hai, to dusri entry allow nahi hogi.

* **Multiple Vehicles Rule:** Agar ek se jyada gaadi aayi hain, toh sabke alag PO numbers allowed nahi hote. Iski ek specific checking (Multiple vehicles with multiple POs is not allowed) ki jati hai.

### 3. Masters Update (Naye Driver ya Vehicle ko save karna)
* Agar gaadi (Vehicle) ya driver pehli baar warehouse aaye hain, toh system automatically unhe apne Master Database (VehicleMaster aur DriverMaster) me save kar leta hai taaki agli baar jaldi processing ho sake.

### 4. Gate Pass banna ya Reject hona (Decision Step)
Sab kuch check hone ke baad, system do me se ek decision leta hai:

* **Gate Pass (Approve):** Agar PO number sahi mil jata hai, toh us entry ka status **"IN_YARD"** ho jata hai. Matlab gaadi ko warehouse ke andar aane ki permission mil gayi. System automatically ek 6-digit ka random alphanumeric **Pass Number** (jaise A5G7K9) generate karke Gate Pass issue kar deta hai.

* **Rejection:** Agar PO numbers match nahi hote ya koi issue nikalta hai, toh entry ka status **"REJECTED"** ho jata hai. System ek Rejection Reason save kar leta hai (jaise "PO validation failed") aur entry block ho jati hai.

### 5. Attachments aur Captures (Photo Save karna)
* Jo photo gate par capture ki gayi thi, usko cloud/file system par secure tarike se upload kiya jata hai. Is uploaded image ke URL ko driver details aur vehicle details ke sath system me hamesha ke liye attach (save) kar diya jata hai.

**Summary me kahen toh:**
Gate Inward bas ek data entry nahi hai. Ye ek chaukidar ki tarah hai jo driver, gaadi aur uske documents verify karta hai, system check lagata hai ki gaadi andar lene layak hai ya nahi, aur sab theek hone par usko **IN_YARD** status de kar ek Gate Pass Number nikal kar deta hai!

---
---
---

# Step 2: Dock Assignment


Lekin usse pehle ek choti si observation: Maine aapke codebase me check kiya toh dekha ki `Wms.Dock.Application` ka folder abhi khali (empty) hai aur Dock ke commands (jaise `AssignDockCommand`) abhi code me implement nahi hue hain. Halanki, Database me iski taiyari ho chuki hai (jaise `TruckQueue` aur `DockDoor` tables ban gaye hain).

Kyunki abhi code nahi likha gaya hai, isliye main aapko iska **Business Process (Logic)** step-by-step detail me batata hu ki ek standard WMS me Dock Assignment kaise kaam karta hai aur iska flow kya hona chahiye:

### Step 1: Yard me Waiting (Queueing)
Jaise hi Gate Inward (First step) complete hota hai, gaadi ka status **"IN_YARD"** ho jata hai. Ab truck warehouse ke parking area (Yard) me khada hota hai aur unloading ki baari ka wait karta hai.
* System is truck ko ek waiting list ya **`TruckQueue`** me daal deta hai.

### Step 2: Dock Availability Check (Khali Dock dhundhna)
Warehouse me alag-alag Docks (Unloading points) hote hain (jaise `DockDoor` table). System ye check karta hai:
* Kaunsa Dock abhi khaali (Available) hai?
* Kya maal (goods) kisi specific type ka hai? (Example: Agar frozen items hain, toh gaadi ko usi Dock par assign karna padega jiske paas cold storage ka rasta hai).
* Kis gaadi ki priority high hai (FIFO rule ya Urgent POs)?

### Step 3: Dock Assign Karna (Assignment Command)
Jab koi appropriate Dock khaali mil jata hai, toh operator system me gaadi ko us dock se link karta hai.
* Yahan ek naya command (jaise `AssignDockCommand`) chalega jo **TruckQueue** me jakar us gaadi ke aage `AssignedDock = "Dock-05"` update kar dega.
* Driver ko ek notification ya parchi (slip) de di jati hai ki: *"Aapko apni gaadi Dock Number 05 par lagani hai."*

### Step 4: Gaadi ko Dock par Lagana (Dock-In / Arrival at Dock)
Driver apni gaadi yard se nikal kar assigned Dock par reverse karke lagata hai.
* Jaise hi gaadi physically lag jati hai, operator system me status update karta hai ki gaadi dock par aagayi (Status change from **IN_YARD** ➔ **AT_DOCK**).

### Step 5: Unloading Shuru Hona
Ab gaadi se physically maal nikalna (unload karna) shuru ho jata hai. Ye step aage chalkar humare 3rd main step **GRN (Goods Receipt Note)** se jud jata hai, jahan utre hue maal ki counting aur system me entry hoti hai.


---
---
---

# Step 3: GRN (Goods Receipt Note)
---
---


### Step 1: Naya GRN Create Karna (`CreateGrnCommand`)
Sabse pehle operator us Purchase Order (PO) ke liye ek naya GRN banata hai.

* **Basic Checks & Setup:** Code check karta hai ki user kis warehouse ka hai. Uske baad PO Number ko database me dhoondhta hai aur wahan se Vendor ki detail (VendorId) nikalta hai.

* **GRN Number Generate hona:** Database ke ek function (`GenerateGrnNumberWH`) ke through ek naya unique GRN number (jaise GRN-WH1-0001) banta hai. Iske baad us GRN ko ek **"DRAFT"** status me save kar diya jata hai, aur usko Gate Inward se link kar diya jata hai.

* **Special Logic (Off-line Items):** Agar PO me kuch aise items hain jinpar rigorous GRN process applicable nahi hai (code me `IsGrnEnabled == false`), toh system unhe physically count kiye bina automatically "100% Accepted" maan kar GRN Line bana deta hai.

### Step 2: GRN Lines (Items) ko Scan/Add karna (`AddGrnLineCommand`)
Ab operator physically boxes ko open karke items ko ginta (count) hai aur unki entry system me karta hai. Ye step GRN ka sabse crucial validation step hai:
* **Item Verification:** Jo SkuId operator scan kar raha hai, code check karta hai ki kya wo SKU us PO me order kiya gaya tha ya nahi?
* **Quantity & Variance Check:** Code check karta hai:
   * **Received Qty:** Total kitna receive hua?
   * **Accepted Qty:** Kitna theek thaak (pass) nikla?
   * **Rejected Qty:** Kitna toot-foot (damaged) nikla?
   * *Validation Rule:* `Accepted Qty` + `Rejected Qty` = `Received Qty` hona hi chahiye. Sath hi, agar order ki quantity se kam aaya hai to status `SHORT`, jyada aaya hai to `EXCESS`, warna `OK` mark kar diya jata hai.
* **Batch Verification (Bohat Important):** GRN me items ko batche me daalna zaruri hota hai. Isme do conditions hoti hain:
   1. **Non-Self-Line Items (Standard FMCG/Goods):** Inke liye Vendor ka `ExternalBatchNumber`, `ManufactureDate` aur `ExpiryDate` daalna zaruri hota hai. Code check karta hai ki Expiry Date hamesha Manufacture date se aage (greater) honi chahiye.
   2. **Self-Line Items (Inhouse/Specific Categories):** Inke liye `ShelfLifeDays` daalna padta hai.
* **System Batch Number:** Is sab ke baad system khud apna ek internal Batch Number (`GenerateBatchNumberWH` dwara) generate karke is record ko database (`GrnLines` aur `Batches` table) me save kar deta hai.

### Step 3: GRN Approval (Approve / Submit)
Jaise hi saare physical items scan hokar `AddGrnLineCommand` ke through add ho jate hain, toh operator us DRAFT GRN ko submit karta hai.
* Ek command chalti hai (jaise `SubmitGrnForApprovalCommand` ya `ApproveGrnCommand`) jo us GRN ka status DRAFT se **APPROVED** kar deti hai. 

Jaise hi GRN approve hota hai, aapka Goods Receipt ka kaam pura ho jata hai. Ab in items ki Quality Check (QC) ya seedha **Putaway** (Bins me rakhna) ki ja sakti hai! 

Kya aap GRN ke process me kisi calculation ya Batching logic ko aur deeply samajhna chahte hain? Ya hum seedha Inward Flow ke aakhri step **Putaway** par chalein?

---
---
---
# Step 4: QC (Quality Check )

Jaise aapne dekha ki baaki steps ke commands available hain, QC (Quality Check) ke maamle me aapke codebase me ek interesting cheez hai. System me ek `Wms.Qc.Application` naam ka folder/project banaya gaya hai, lekin usme **abhi koi code nahi hai (wo empty hai)**. 

Lekin iska matlab ye nahi ki QC nahi hota. Aapke system me QC ko ek alag step banane ke bajaye, use **GRN aur Putaway ke andar hi mix (embed)** kar diya gaya hai. 

Aaiye QC process ko step-by-step samajhte hain ki aapke system me ye kaise kaam kar raha hai:

### Step 1: First QC (GRN ke Time par)
Jaise hi truck se maal utarta hai aur **GRN (Goods Receipt Note)** banta hai, QC ka pehla step wahin hota hai. (Jo humne `AddGrnLineCommand` me dekha tha).
* Jab operator item ko scan karta hai, to wo item ki physical condition check karta hai.
* Agar item sahi hai, toh uski ginti **`AcceptedQty`** (Pass) me daal deta hai.
* Agar item toota-foota (damaged) hai, ya expiry date paas hai, toh usko **`RejectedQty`** (Fail) me daal deta hai.
* System make sure karta hai ki `AcceptedQty` + `RejectedQty` milakar Total Received quantity ke barabar hone chahiye. Jo reject hua, wo inventory ka part nahi banta.

### Step 2: Second QC (Putaway ke Time par)
Maan lijiye GRN ke waqt item theek lag raha tha (ya andhere me thik se check nahi hua). Ab worker us item ko warehouse ke andar kisi rack/bin par rakhne (Putaway) jaa raha hai.
* Agar worker ko bin par rakhte waqt pata chalta hai ki item me koi damage hai, toh wo system me **`ReportPutawayDamageCommand`** run karta hai.
* Ye command ek secondary QC check ki tarah kaam karti hai.

### Step 3: Rejected / Damaged Items ka kya hota hai? (RTV Process)
Jab bhi `ReportPutawayDamageCommand` chalta hai, system chupchaap damage ko record karke nahi baithta, balki kuch important actions leta hai:
1. **Rejection Record:** System `PutawayRejections` table me ek entry daal deta hai ki kaunsa item, kis bin par, kis reason (ReasonUuid) se reject hua.
2. **Damaged Bin:** Is item ko normal inventory me na daalkar, ek Damaged Bin (ya virtual damaged location) me update kar deta hai (`IsDamagedBin: true`).
3. **Automatic RTV (Return To Vendor):** Sabse important cheez! Code automatically ek aur command fire karta hai `CreateRtvCommand` (CreateRtvForRejection method ke through). Ye vendor ke naam par ek RTV bana deta hai jiska status `PUTAWAY_DAMAGE` likha hota hai, jisse ki kharab maal Vendor ko wapas bheja ja sake.

----
---
---


# Step 5: Putaway


### Step 1: Putaway Task banna aur Allocate hona
Jab GRN (pichla step) pura aur approve ho jata hai, toh system ko pata lag jata hai ki kitna maal warehouse me aaya hai. 
Ab system khud se rules lagata hai (`PutawayRule` commands ke through) ya fir manual allocation ke through ye decide karta hai ki kaunsa item kis Zone, kis Aisle, aur kis **Bin** (rack ka box) me rakha jayega.
* System ek **Task / Assignment** create kar deta hai aur kisi worker ko assign kar deta hai. 
* System worker ko screen par dikhata hai: *"Aapko SKU-123 ke 50 items utha kar Bin A-01 me rakhne hain."*

### Step 2: Physical Movement (Worker ka kaam)
Worker GRN area se wo 50 items apni trolley (pallet) par load karta hai aur batayi gayi location (Bin A-01) ki taraf physically move karta hai.

### Step 3: Putaway Confirm Karna (`ConfirmPutawayCommand`)
Jab worker us Bin (A-01) par pahunch jata hai, toh wo apne device me items ko physically rakh kar **Confirm** karta hai. Aapke code me ye command chalne par ye validations hoti hain:
1. **Bin Validation:** System check karta hai ki worker ne jo Bin scan kiya hai, kya wo sahi hai?
2. **Batch & SKU Validation:** Kya worker wahi batch aur wahi SKU rakh raha hai jo usko bataya gaya tha?
3. **Quantity Validation:** System check karta hai ki kya worker limit se jyada quantity toh us bin me nahi daal raha? (Code check: `if (dto.Qty > allocation.RemainingQuantity) throw error`).

### Step 4: Inventory ko "Available" karna (Sabse Important Step!)
Agar validation pass ho jati hai, toh system Database me `LocationInventoryBatches` table me jata hai.
* Pehle ye check karta hai ki us Bin me is item ka pehle se kitna stock hai.
* Phir abhi worker ne jitna count rakha hai, usko add karke **`AvailableQty`** ko badha deta hai.
* Agar item kisi kaaran (damage) ki wajah se kharab nikalta hai, to worker wahin par use Damaged/Rejected mark kar sakta hai (`ReportPutawayDamageCommand` jo humne QC step me samjha tha).

### Step 5: Task ko Complete karna
Akhir me system check karta hai (using `ReconcileAssignmentAsync` method) ki kya worker ne saara quantity rakh diya? Agar `RemainingQuantity` zero (0) ho jati hai, toh us assignment ko **"COMPLETED"** mark kar diya jata hai.

**Connection to Outward:**
Jaise hi Step 4 me `AvailableQty` badhti hai, iska matlab hai apka **Inward Flow puri tarah se khatam ho gaya!** Ab ye items Customer ko bhejne (Outward Flow) ke liye poori tarah se ready aur live hain. 

Aapka Inward process **Gate (Entry) ➔ Dock ➔ GRN ➔ QC ➔ Putaway (Bin me rakhna)** par yahan aakar officially close hota hai. 

Kya aapko ab pure Inward flow ki clear picture mil gayi hai, ya isme kisi point par kuch aur doubt hai?




