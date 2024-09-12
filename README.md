# React + TypeScript + Vite
## CT648-Bun_React_Frontend_Web
## โปรเจคนี้เป็นการเรียนรู้การใช้ React + TypeScript + Bun และการใช้งาน Public Rest API
### หลักการออกแบบ
#### ออกแบบให้หน้าเว็บแสดง 3 ส่วน คือ
1. DataDisplay จะจัดการการแสดงผลข้อมูลมุขตลก โดยการดึงข้อมูลจาก API และแสดงข้อมูล Joke แบบสุ่มพร้อมกับรูปภาพโปเกมอน ใช้ useEffect เพื่อทำให้คอมโพเนนต์อัปเดต joke ทุกๆ 15 วินาที
2. PokemonTable จะจัดการการแสดงผลข้อมูลตัวอย่างชื่อ Pokémon ในรูปแบบตาราง
3. DataDisplay_pok จะจัดการการดึงและแสดงข้อมูลเกี่ยวกับ Pokémon จาก PokéAPI โดยมีช่องให้กรอกชื่อ Pokémon แล้วกด Search ก็จะโชว์รูปและข้อมูลของ Pokémon

#### ขั้นตอนการสร้าง
1. สร้างโปรเจกต์ React ด้วย Vite โดยใช้ bun ด้วยคำสั่ง
   ```bash
   bun create vite my-react-app --template react-ts
   ```
2. เปลี่ยนตำแหน่งไปที่โฟลเดอร์ my-react-app ที่เพิ่งสร้างขึ้น
   ```bash   
   cd my-react-app
   ```
3. ติดตั้งไลบรารี Axios ในโปรเจกต์ที่ใช้ Bun เป็นตัวจัดการแพ็กเกจที่ช่วยให้คุณทำคำขอ HTTP ได้ง่ายขึ้น (เช่น GET, POST) เพื่อเชื่อมต่อกับ API หรือเว็บเซอร์วิสอื่นๆ
   ```bash   
   bun add axios
   ```
4. สร้างโฟลเดอร์ชื่อ components ภายในโฟลเดอร์ src และสร้างไฟล์ชื่อ DataDisplay.tsx  , PokemonTable.tsx  , DataDisplay_pok.tsx  ภายในโฟลเดอร์ src/components
   - คำสั่งสร้างโฟลเดอร์
   ```bash   
   mkdir src/components
   ```
   - คำสั่งสร้างไฟล์สำหรับ Unix/Linux หรือ MacOS
   ```bash   
   touch src/components/DataDisplay.tsx
   ```
   - คำสั่งสร้างไฟล์สำหรับ Windows
   ```bash   
   New-Item -Path "src/components/DataDisplay.tsx" -ItemType File
   ```
5. สร้างโค้ดไว้ในไฟล์ src/components/DataDisplay.tsx เป็นคอมโพเนนต์ React ที่ใช้ TypeScript สำหรับแสดงข้อมูล Joke จาก API แบบสุ่ม โดยใช้ Axios สำหรับทำคำขอ HTTP เพื่อดึงข้อมูลจาก API ภายนอก และยังมีการใช้รูปภาพและไฟล์ CSS เพื่อทำการตกแต่งคอมโพเนนต์
   - useState: ใช้ในการจัดการสถานะของข้อมูลที่เปลี่ยนแปลงได้ในคอมโพเนนต์ เช่น ข้อมูลมุขตลกและสถานะการโหลด
   ```bash   
   const [data, setData] = useState<JokeData | null>(null); // สถานะข้อมูลมุขตลก
   const [loading, setLoading] = useState<boolean>(false);  // สถานะการโหลด
   ```
   - ดึงข้อมูลจาก API และแสดงข้อมูล Joke แบบสุ่มพร้อมกับรูปภาพโปเกมอน จาก https://official-joke-api.appspot.com/random_joke
   ```bash 
   const fetchData = async () => {
     setLoading(true); // แสดงสถานะกำลังโหลดข้อมูล
     try {
       const response = await axios.get("https://official-joke-api.appspot.com/random_joke");
       setData(response.data); // ตั้งค่าข้อมูลมุขตลกที่ได้รับจาก API
     } catch (error) {
       console.error("Error fetching data:", error); // แสดงข้อผิดพลาดถ้ามีปัญหา
       setData(null); // ถ้าเกิดข้อผิดพลาดให้ตั้งค่าข้อมูลเป็น null
     } finally {
       setLoading(false); // ปิดสถานะการโหลดหลังจากดึงข้อมูลเสร็จ
     }
   };
   ```
   - ใช้ useEffect เพื่อทำให้คอมโพเนนต์อัปเดต joke ทุกๆ 15 วินาที
   ```bash 
   useEffect(() => {
     fetchData(); // เรียกข้อมูลครั้งแรกเมื่อคอมโพเนนต์ถูกโหลด

     const intervalId = setInterval(() => {
       fetchData(); // เรียกข้อมูลทุก 15 วินาที
     }, 15000);

     return () => clearInterval(intervalId); // เคลียร์ interval เมื่อคอมโพเนนต์ถูก unmounted
   }, []);
   ```
6. สร้างโค้ดไว้ในไฟล์ src/components/PokemonTable.tsx โค้ดนี้เป็นคอมโพเนนต์ React ที่ใช้ในการแสดงตัวอย่างรายชื่อ Pokémon โดยใช้ตาราง HTML เพื่อจัดระเบียบข้อมูล
7.	สร้างโค้ดไว้ในไฟล์ src/components/DataDisplay_pok.tsx เป็นคอมโพเนนต์ React ที่ใช้ในการดึงและแสดงข้อมูลเกี่ยวกับ Pokémon โดยใช้ PokéAPI และจัดการข้อมูลเหล่านั้นให้เป็นรูปแบบที่สามารถแสดงผลได้ ช่วยให้ผู้ใช้สามารถค้นหาข้อมูล Pokémon โดยการป้อนชื่อ Pokémon และดูข้อมูลที่เกี่ยวข้องได้ทันที
   - ใช้ useState เพื่อจัดการ state สามตัว:
        - data: เก็บข้อมูลโปเกมอนที่ถูกดึงมาจาก API
        - pokemonName: เก็บชื่อโปเกมอนที่ผู้ใช้ต้องการค้นหา
        - loading: เก็บสถานะการโหลดข้อมูล
      ```bash
      const DataDisplay_pok = () => {
        const [data, setData] = useState<PokemonData | null>(null);
        const [pokemonName, setPokemonName] = useState<string>("pikachu");
        const [loading, setLoading] = useState<boolean>(false);
      };
      ```
   - ใช้ฟังก์ชันดึงข้อมูล fetchData และใช้ ใช้ axios เพื่อดึงข้อมูลโปเกมอนจาก API ของ PokeAPI โดยใช้ชื่อโปเกมอนที่ผู้ใช้ป้อน
        - ฟังก์ชันนี้จะถูกเรียกเมื่อผู้ใช้ค้นหาโปเกมอนใหม่หรือเมื่อเริ่มโหลดคอมโพเนนต์
        - ดึงข้อมูลจาก API โดยใช้ชื่อโปเกมอน และเก็บข้อมูลใน state data
        - จัดการสถานะการโหลดและแสดงผลหากมีข้อผิดพลาด
      ```bash
     const fetchData = async (name: string) => {
       setLoading(true);
       try {
         const response = await axios.get(
           `https://pokeapi.co/api/v2/pokemon/${name}`
         );
         setData(response.data);
       } catch (error) {
         console.error("Error fetching data:", error);
         setData(null);
       } finally {
         setLoading(false);
       }
     };
      ```
   - ใช้ useEffect และการส่งข้อมูลผ่านฟอร์ม
        - useEffect ใช้เพื่อดึงข้อมูลทันทีที่มีการเปลี่ยนแปลงชื่อโปเกมอน
        - การส่งข้อมูลผ่านฟอร์มใช้ฟังก์ชัน handleSubmit เพื่อดึงข้อมูลตามชื่อโปเกมอนที่ผู้ใช้ป้อน
      ```bash
      useEffect(() => {
        fetchData(pokemonName);  // เรียก fetchData เมื่อ pokemonName เปลี่ยนแปลง
      }, [pokemonName]);

      const handleSubmit = (e: React.FormEvent) => {
        e.preventDefault();  // ป้องกันการ reload หน้าเว็บเมื่อส่งฟอร์ม
        fetchData(pokemonName.toLowerCase());  // ดึงข้อมูลโปเกมอนที่ป้อน
      };
      ```
8.	แก้ไขโค้ดที่ src/App.tsx เป็นคอมโพเนนต์หลักของแอปพลิเคชัน React ของคุณ ซึ่งจะรวมคอมโพเนนต์อื่นๆ ที่ได้สร้างไว้เข้าด้วยกันเพื่อให้แสดงผลในหน้าเดียวกัน
   - DataDisplay จะจัดการการแสดงผลข้อมูลมุขตลก
   - PokemonTable จะจัดการการแสดงผลข้อมูลตัวอย่างชื่อ Pokémon ในรูปแบบตาราง
   - DataDisplay_pok จะจัดการการดึงและแสดงข้อมูลเกี่ยวกับ Pokémon จาก PokéAPI
      ```bash
      import DataDisplay from "./components/DataDisplay";
      import DataDisplay_pok from "./components/DataDisplay_pok";
      import PokemonTable from "./components/PokemonTable";
      ```

##### ผู้จัดทำ
##### 66130136	จิราพร สอนบุญมา
##### 66130503	อารักษ์ ลิบน้อย
##### 66130662	หทัยรัตน์ เจนวิทยา
