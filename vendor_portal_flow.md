# Vendor Portal Business Flow

Yeh document Vendor Portal ka actual functional flow explain karta hai, ki business aur operations level par cheezein kaise kaam karti hain.

## 1. Vendor/Seller Onboarding
- Jab koi naya vendor humare sath judta hai, toh uska pura profile system mein banaya jata hai.
- **Business Details**: Unka legal naam, trade name, aur basic contact info system mein record hoti hai.
- **Compliance**: Unka PAN, GSTIN aur FSSAI (food license) verify aur store hota hai. Portal itna smart hai ki GST number ke shuru ke 2 digits se vendor ka State automatic pata laga leta hai.
- **Bank Info**: Vendor ki bank details (Account Number, IFSC, etc.) store hoti hain taaki aage unki payments sahi jagah process ki ja sakein.

## 2. Buyer aur Seller ka Connection
- Sirf vendor ka portal par hona kaafi nahi hai. Ek seller ko kisi specific Buyer (hamari company ka entity) ke sath link hona padta hai.
- Is link ke banne par business terms tay hoti hain, jaise ki seller ko kitne din ka credit milega (udhaar kab chukana hai) aur payment terms kya hongi.
- **Approval Process**: Naye relation turant start nahi hote. Pehle internal team (jaise Finance ya Legal) se review aur approval chahiye hota hai. Jab vendor status "Active" hota hai, tabhi usko naye order (PO) diye ja sakte hain.
- Financial accounting sahi rahe isliye is relation ko ERP system (jaise SAP ya Zoho) ke codes se bhi map kiya jata hai.

## 3. Purchase Order (PO) Banana (Creation)
Jab kisi buyer ko vendor se material lena hota hai, toh wo PO raise karta hai. Iska process kuch is tarah hai:

- **Addresses (Bohot Zaroori)**: PO banate waqt location details ekdum sateek honi chahiye. Tax aur invoice ke compliance ke liye chaar (4) locations hona mandatory hai:
  1. Humara (Buyer ka) Billing address (GST wala address).
  2. Delivery kahan karni hai (Humara Shipping address).
  3. Vendor kis godown se saman bhejega (Vendor Dispatch location).
  4. Vendor ka tax address kya hai.
- **Items & Pricing**: PO mein items add kiye jate hain jinki exact quantity aur price tay hota hai, aur system taxes (GST) automatic calculate karta hai.
- **Duplicate Check**: Galti se do baar same order na chala jaye isliye system safety check karta hai ki agar us user ne same vendor ko same items ka order pichle 1 ghante mein diya hai toh wo ek error/warning dega.
- **Emergency PO**: Agar bohot urgent kaam hai aur formal Quotation (RFQ) ka process wait nahi kar sakte, toh ek Emergency PO banaya ja sakta hai. Isme system mangta hai ki 'Kyu urgent hai?' aur user ki spending limits (jaise manager ko 10,000 allow hai) ke hisaab se order block ya pass karta hai.

## 4. PO ka Safar (Lifecycle)
Ek baar PO form hone ke baad wo kayi stages se guzarta hai:

1. **Draft Stage**: Start mein PO ek kachha draft hota hai. Check karne ke baad isko aage push kiya jata hai.
2. **Internal Approval**: Company ka head/manager ise approve karta hai. Agar unko lagta hai changes hone chahiye, toh wo "Revision Required" mark karte hain.
3. **Vendor ki Acceptance**: Humare approval ke baad, PO vendor ko visible hota hai. Vendor ko ise "Accept" karna padta hai ki wo is daam par ye saman de sakta hai.
4. *(Optional) Advance Payment*: Agar tay hua tha ki vendor ko advance chaiye, toh PO accept hone ke baad "Pending Advance" state me ruka rehta hai jab tak Finance team advance clear nahi karti.
5. **Dispatch & Arrive**: Vendor wahan se truck nikalta hai ("In Transit") aur jab saman warehouse ke gate par aata hai toh status "Arrived at Gate" mark hota hai.
6. **Delivery & GRN**: Warehouse team items physically ginti hai aur GRN (Goods Receipt Note) banati hai. Agar thoda hi saman aya toh "Partially Delivered", aur pura gaya toh "Delivered". Isse system nikalta hai ki vendor ne kitna percent order poora kiya (Fill Rate report).
7. **Closure**: Delivery aur Payment complete hone par PO formally Close/Complete ho jata hai.

(Note: Agar order kisi service, labour, ya rent ki machine ke liye tha, toh lifecycle thodi alag hoti hai jaise daily timesheet approval ya machine uptime check karna, par fundamental concept same hi rehta hai.)
