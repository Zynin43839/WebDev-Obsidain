# Geospatial Development By Example with Python

> **Author:** Pablo Carleo, Daniela Quangoli

## สรุปเนื้อหาหนังสือ

หนังสือเล่มนี้สอนการพัฒนาแอปพลิเคชันทางภูมิศาสตร์ (Geospatial) ด้วย Python แบบ hands-on ตั้งแต่เริ่มต้นจนถึงขั้นสูง ครอบคลุม 10 บท เริ่มจากการสร้างแอป geocaching ง่ายๆ ไปจนถึงการประมวลผลภาพดาวเทียมแบบ parallel processing

---

## Chapter 1 – การเตรียมสภาพแวดล้อม (Environment Setup)

บอกรายละเอียดการติดตั้ง Python, IDE (PyCharm), และเครื่องมือที่จำเป็นสำหรับการพัฒนาแอป geospatial แนะนำให้ใช้ virtual environment สำหรับจัดการ dependencies แต่ละโปรเจกต์แยกกัน

---

## Chapter 2 – แอปพลิเคชัน Geocaching (Geocaching App)

เริ่มต้นสร้างแอปจัดการข้อมูล geocaching แบบง่ายๆ โดยใช้ข้อมูล GPX file อ่านพิกัดละติจูด/ลองจิจูดจากไฟล์ แล้วแสดงผลบนแผนที่ ใช้ไลบรารีอย่าง xmltodict สำหรับ parse XML จากนั้นพัฒนาเป็น desktop app ที่สามารถค้นหาและกรองข้อมูลจุด geocaching ได้

---

## Chapter 3 – แหล่งข้อมูลทางภูมิศาสตร์ (Data Sources)

แนะนำแหล่งข้อมูล geospatial ฟรี เช่น OpenStreetMap, Natural Earth, และ USGS สาธิตการดาวน์โหลดและใช้ข้อมูลในรูปแบบต่างๆ เช่น shapefile, GPX, KML อธิบายความแตกต่างระหว่าง vector data และ raster data พร้อมตัวอย่างการใช้งานจริง

---

## Chapter 4 – การปรับปรุงการค้นหาของแอป (Search Capabilities)

พัฒนาความสามารถในการค้นหาของแอป geocaching โดยใช้หลักการทางคณิตศาสตร์ เช่น สูตร Haversine สำหรับคำนวณระยะทางระหว่างสองจุด สร้าง spatial index เพื่อเร่งการค้นหา ใช้ Shapely library สำหรับสร้างและจัดการ geometry objects เช่น Point, LineString, Polygon พร้อมใช้ predicate ต่างๆ เช่น contains, intersects, within สำหรับกรองข้อมูลเชิงพื้นที่

---

## Chapter 5 – การออกแบบเชิงวัตถุ (OOP Design)

 refactor โค้ดจากบทก่อนหน้าให้เป็น Object-Oriented Design ที่สะอาดและ maintainable ได้มากขึ้น สร้างคลาสสำหรับจัดการข้อมูล geospatial โดยเฉพาะ ใช้ OGR (part of GDAL) สำหรับอ่านข้อมูลจาก vector file ต่างๆ เช่น shapefile, GeoJSON, GML สาธิตการสร้างแผนที่ด้วย Mapnik library ทั้งแบบ XML stylesheet และแบบ Python API รวมถึงการจัดวาง label บนแผนที่

---

## Chapter 6 – ข้อมูล Raster (Raster Data)

แนะนำ raster data คือข้อมูลที่อยู่ในรูปแบบ pixel grid เช่น ภาพดาวเทียม DEM (Digital Elevation Model) อธิบายแนวคิดพื้นฐาน เช่น bounding box, coordinate systems, bands, pixel values สาธิตการอ่านและจัดการ raster ด้วย GDAL ใช้ OpenCV สำหรับการแสดงผล image ครอบคลุมทั้ง simple raster เช่น elevation data และ remote sensing images จากดาวเทียม Landsat

---

## Chapter 7 – การประมวลผล Raster (Raster Processing)

สอนเทคนิคการประมวลผลข้อมูล raster เช่น การจำแนกประเภท pixel (classification) การคำนวณสถิติพื้นฐานจากข้อมูล raster เช่น min, max, mean, histogram ใช้ NumPy สำหรับคำนวณบน array ขนาดใหญ่ สาธิตการสร้าง elevation profile และ cross-section จาก DEM data อธิบาย GeoTIFF format และวิธีการสร้าง/แก้ไขไฟล์ GeoTIFF

---

## Chapter 8 – แอป Data Miner (Data Miner App)

สร้างแอปพลิเคชัน Data Miner ที่ใช้ Django framework สำหรับจัดการฐานข้อมูลเชิงพื้นที่ ใช้ Django GIS สำหรับเก็บและ query ข้อมูล geometry ใน PostgreSQL/PostGIS สาธิตการ import ข้อมูลจาก shapefile, GPX, GML เข้าสู่ฐานข้อมูล ใช้ GeoManager สำหรับจัดการ collection ของข้อมูลทางภูมิศาสตร์ แสดงวิธี optimize database insert ด้วย bulk_create เพื่อเพิ่มความเร็ว พร้อมดึงข้อมูล POI จาก OpenStreetMap ผ่าน Overpass API

---

## Chapter 9 – การประมวลผลภาพขนาดใหญ่ (Processing Big Images)

จัดการกับปัญหา memory consumption เมื่อทำงานกับภาพดาวเทียมขนาดใหญ่ สาธิตว่าการ open image ทั้งหมดพร้อมกันทำให้ memory เต็มอย่างรวดเร็ว แก้ปัญหาด้วย technique ที่เรียกว่า chunk-based reading คืออ่านภาพทีละส่วนเล็กๆ แล้วค่อยๆ ประมวลผล ใช้ GDAL `ReadAsArray()` สำหรับอ่าน specific region จากภาพ ใช้ Python iterators และ generators สำหรับ iterate ทีละ row โดยไม่ต้องโหลดทั้งภาพเข้า memory สาธิตการสร้าง true color composition และ false color composition จาก Landsat 8 bands

---

## Chapter 10 – การประมวลผลแบบ Parallel (Parallel Processing)

ใช้ Python multiprocessing library สำหรับกระจายงานไปยัง CPU cores หลายตัวพร้อมกัน อธิบาย GIL (Global Interpreter Lock) ใน CPython และวิธีหลีกเลี่ยงด้วย multiprocessing สาธิต block iteration สำหรับอ่านภาพเป็น block ขนาดเท่ากัน พร้อมปรับให้เข้ากับ native block size ของไฟล์ TIFF เทคนิค image resampling สำหรับเปลี่ยนความละเอียดของภาพด้วย interpolation methods ต่างๆ เช่น nearest, linear, bicubic, lanczos สอน pan sharpening คือการผสม band ความละเอียดสูง (panchromatic) เข้ากับ color bands เพื่อเพิ่มความคมชัดของภาพ สุดท้ายสรุปว่า parallel processing เหมาะกับ CPU-bound tasks แต่อาจไม่ช่วยเมื่อ hardware bottleneck อยู่ที่ disk I/O

---

## เครื่องมือและไลบรารีหลักที่ใช้

- **GDAL/OGR** – อ่าน/เขียน vector และ raster data
- **Shapely** – สร้างและจัดการ geometry objects
- **Mapnik** – สร้างแผนที่คุณภาพสูง
- **NumPy** – คำนวณ array ขนาดใหญ่
- **OpenCV (cv2)** – ประมวลผลภาพ
- **Django + PostGIS** – เว็บแอปพร้อมฐานข้อมูลเชิงพื้นที่
- **xmltodict** – parse XML files (GPX, KML)
- **overpy** – ดึงข้อมูลจาก OpenStreetMap
- **multiprocessing** – parallel processing

---

## กลุ่มเป้าหมาย

หนังสือเหมาะสำหรับนักพัฒนา Python ที่ต้องการเริ่มต้นทำงานกับข้อมูลเชิงพื้นที่ (geospatial data) ไม่จำเป็นต้องมีพื้นฐาน GIS มาก่อน แต่ควรมีความรู้ Python พื้นฐาน หนังสือเน้นการเรียนรู้แบบ project-based ทุกบทมี code example ให้ลองทำตามจริง
