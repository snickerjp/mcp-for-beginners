<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "7a11a5dcf2f9fdf6392f5a4545cf005e",
  "translation_date": "2025-06-11T16:05:29+00:00",
  "source_file": "05-AdvancedTopics/web-search-mcp/README.md",
  "language_code": "id"
}
-->
# Lesson: Membangun Server MCP Pencarian Web

Bab ini menunjukkan cara membangun agen AI dunia nyata yang terintegrasi dengan API eksternal, menangani berbagai jenis data, mengelola kesalahan, dan mengatur beberapa alat—semua dalam format siap produksi. Anda akan melihat:

- **Integrasi dengan API eksternal yang memerlukan autentikasi**
- **Menangani berbagai jenis data dari beberapa endpoint**
- **Strategi penanganan kesalahan dan pencatatan yang tangguh**
- **Orkestrasi multi-alat dalam satu server**

Di akhir, Anda akan memiliki pengalaman praktis dengan pola dan praktik terbaik yang penting untuk aplikasi AI dan LLM tingkat lanjut.

## Pendahuluan

Dalam pelajaran ini, Anda akan belajar cara membangun server MCP dan klien canggih yang memperluas kemampuan LLM dengan data web waktu nyata menggunakan SerpAPI. Ini adalah keterampilan penting untuk mengembangkan agen AI dinamis yang dapat mengakses informasi terkini dari web.

## Tujuan Pembelajaran

Di akhir pelajaran ini, Anda akan mampu:

- Mengintegrasikan API eksternal (seperti SerpAPI) secara aman ke dalam server MCP
- Mengimplementasikan beberapa alat untuk pencarian web, berita, produk, dan tanya jawab
- Mengurai dan memformat data terstruktur untuk konsumsi LLM
- Menangani kesalahan dan mengelola batasan API secara efektif
- Membangun dan menguji klien MCP otomatis dan interaktif

## Server MCP Pencarian Web

Bagian ini memperkenalkan arsitektur dan fitur Server MCP Pencarian Web. Anda akan melihat bagaimana FastMCP dan SerpAPI digunakan bersama untuk memperluas kemampuan LLM dengan data web waktu nyata.

### Gambaran Umum

Implementasi ini menampilkan empat alat yang menunjukkan kemampuan MCP untuk menangani tugas berbasis API eksternal yang beragam secara aman dan efisien:

- **general_search**: Untuk hasil web umum
- **news_search**: Untuk berita terkini
- **product_search**: Untuk data e-commerce
- **qna**: Untuk potongan tanya jawab

### Fitur
- **Contoh Kode**: Menyertakan blok kode spesifik bahasa untuk Python (dan mudah diperluas ke bahasa lain) menggunakan bagian yang dapat dilipat demi kejelasan

<details>  
<summary>Python</summary>  

```python
# Example usage of the general_search tool
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("general_search", arguments={"query": "open source LLMs"})
            print(result)
```
</details>

Sebelum menjalankan klien, ada baiknya memahami apa yang dilakukan server. File [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py) file implements the MCP server, exposing tools for web, news, product search, and Q&A by integrating with SerpAPI. It handles incoming requests, manages API calls, parses responses, and returns structured results to the client.

You can review the full implementation in [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py).

Berikut contoh singkat bagaimana server mendefinisikan dan mendaftarkan sebuah alat:

<details>  
<summary>Server Python</summary> 

```python
# server.py (excerpt)
from mcp.server import MCPServer, Tool

async def general_search(query: str):
    # ...implementation...

server = MCPServer()
server.add_tool(Tool("general_search", general_search))

if __name__ == "__main__":
    server.run()
```
</details>

- **Integrasi API Eksternal**: Menunjukkan penanganan aman kunci API dan permintaan eksternal
- **Penguraian Data Terstruktur**: Menampilkan cara mengubah respons API menjadi format yang ramah LLM
- **Penanganan Kesalahan**: Penanganan kesalahan yang kuat dengan pencatatan yang sesuai
- **Klien Interaktif**: Menyertakan pengujian otomatis dan mode interaktif untuk pengujian
- **Manajemen Konteks**: Memanfaatkan MCP Context untuk pencatatan dan pelacakan permintaan

## Prasyarat

Sebelum memulai, pastikan lingkungan Anda sudah diatur dengan benar dengan mengikuti langkah-langkah ini. Ini akan memastikan semua dependensi terpasang dan kunci API Anda dikonfigurasi dengan benar untuk pengembangan dan pengujian yang lancar.

- Python 3.8 atau lebih tinggi
- Kunci API SerpAPI (Daftar di [SerpAPI](https://serpapi.com/) - tersedia tier gratis)

## Instalasi

Untuk memulai, ikuti langkah-langkah berikut untuk mengatur lingkungan Anda:

1. Instal dependensi menggunakan uv (disarankan) atau pip:

```bash
# Using uv (recommended)
uv pip install -r requirements.txt

# Using pip
pip install -r requirements.txt
```

2. Buat file `.env` di root proyek dengan kunci SerpAPI Anda:

```
SERPAPI_KEY=your_serpapi_key_here
```

## Penggunaan

Server MCP Pencarian Web adalah komponen inti yang menyediakan alat untuk pencarian web, berita, produk, dan tanya jawab dengan integrasi SerpAPI. Server ini menangani permintaan masuk, mengelola panggilan API, mengurai respons, dan mengembalikan hasil terstruktur ke klien.

Anda dapat melihat implementasi lengkap di [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py).

### Menjalankan Server

Untuk memulai server MCP, gunakan perintah berikut:

```bash
python server.py
```

Server akan berjalan sebagai server MCP berbasis stdio yang dapat langsung dihubungkan oleh klien.

### Mode Klien

Klien (`client.py`) supports two modes for interacting with the MCP server:

- **Normal mode**: Runs automated tests that exercise all the tools and verify their responses. This is useful for quickly checking that the server and tools are working as expected.
- **Interactive mode**: Starts a menu-driven interface where you can manually select and call tools, enter custom queries, and see results in real time. This is ideal for exploring the server's capabilities and experimenting with different inputs.

You can review the full implementation in [`client.py`](../../../../05-AdvancedTopics/web-search-mcp/client.py).

### Menjalankan Klien

Untuk menjalankan pengujian otomatis (ini akan otomatis memulai server):

```bash
python client.py
```

Atau jalankan dalam mode interaktif:

```bash
python client.py --interactive
```

### Pengujian dengan Metode Berbeda

Ada beberapa cara untuk menguji dan berinteraksi dengan alat yang disediakan oleh server, tergantung kebutuhan dan alur kerja Anda.

#### Menulis Skrip Uji Kustom dengan MCP Python SDK
Anda juga dapat membuat skrip uji sendiri menggunakan MCP Python SDK:

<details>
<summary>Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def test_custom_query():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            # Call tools with your custom parameters
            result = await session.call_tool("general_search", 
                                           arguments={"query": "your custom query"})
            # Process the result
```
</details>

Dalam konteks ini, "skrip uji" berarti program Python kustom yang Anda tulis untuk bertindak sebagai klien bagi server MCP. Alih-alih menjadi unit test formal, skrip ini memungkinkan Anda terhubung secara programatik ke server, memanggil alat apa pun dengan parameter yang Anda pilih, dan memeriksa hasilnya. Pendekatan ini berguna untuk:
- Prototipe dan eksperimen dengan panggilan alat
- Memvalidasi bagaimana server merespon input berbeda
- Mengotomatisasi pemanggilan alat berulang
- Membangun alur kerja atau integrasi sendiri di atas server MCP

Anda dapat menggunakan skrip uji untuk mencoba kueri baru dengan cepat, debug perilaku alat, atau bahkan sebagai titik awal untuk otomasi yang lebih maju. Berikut contoh cara menggunakan MCP Python SDK untuk membuat skrip seperti itu:

## Deskripsi Alat

Anda dapat menggunakan alat berikut yang disediakan oleh server untuk melakukan berbagai jenis pencarian dan kueri. Setiap alat dijelaskan di bawah dengan parameter dan contoh penggunaannya.

Bagian ini memberikan detail tentang setiap alat yang tersedia dan parameternya.

### general_search

Melakukan pencarian web umum dan mengembalikan hasil yang diformat.

**Cara memanggil alat ini:**

Anda dapat memanggil `general_search` dari skrip Anda sendiri menggunakan MCP Python SDK, atau secara interaktif menggunakan Inspector atau mode klien interaktif. Berikut contoh kode menggunakan SDK:

<details>
<summary>Contoh Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_general_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("general_search", arguments={"query": "latest AI trends"})
            print(result)
```
</details>

Alternatifnya, dalam mode interaktif, pilih `general_search` from the menu and enter your query when prompted.

**Parameters:**
- `query` (string): Kuery pencarian

**Contoh Permintaan:**

```json
{
  "query": "latest AI trends"
}
```

### news_search

Mencari artikel berita terbaru terkait kueri.

**Cara memanggil alat ini:**

Anda dapat memanggil `news_search` dari skrip Anda sendiri menggunakan MCP Python SDK, atau secara interaktif menggunakan Inspector atau mode klien interaktif. Berikut contoh kode menggunakan SDK:

<details>
<summary>Contoh Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_news_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("news_search", arguments={"query": "AI policy updates"})
            print(result)
```
</details>

Alternatifnya, dalam mode interaktif, pilih `news_search` from the menu and enter your query when prompted.

**Parameters:**
- `query` (string): Kuery pencarian

**Contoh Permintaan:**

```json
{
  "query": "AI policy updates"
}
```

### product_search

Mencari produk yang sesuai dengan kueri.

**Cara memanggil alat ini:**

Anda dapat memanggil `product_search` dari skrip Anda sendiri menggunakan MCP Python SDK, atau secara interaktif menggunakan Inspector atau mode klien interaktif. Berikut contoh kode menggunakan SDK:

<details>
<summary>Contoh Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_product_search():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("product_search", arguments={"query": "best AI gadgets 2025"})
            print(result)
```
</details>

Alternatifnya, dalam mode interaktif, pilih `product_search` from the menu and enter your query when prompted.

**Parameters:**
- `query` (string): Kuery pencarian produk

**Contoh Permintaan:**

```json
{
  "query": "best AI gadgets 2025"
}
```

### qna

Mendapatkan jawaban langsung untuk pertanyaan dari mesin pencari.

**Cara memanggil alat ini:**

Anda dapat memanggil `qna` dari skrip Anda sendiri menggunakan MCP Python SDK, atau secara interaktif menggunakan Inspector atau mode klien interaktif. Berikut contoh kode menggunakan SDK:

<details>
<summary>Contoh Python</summary>

```python
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_qna():
    server_params = StdioServerParameters(
        command="python",
        args=["server.py"],
    )
    async with stdio_client(server_params) as (reader, writer):
        async with ClientSession(reader, writer) as session:
            await session.initialize()
            result = await session.call_tool("qna", arguments={"question": "what is artificial intelligence"})
            print(result)
```
</details>

Alternatifnya, dalam mode interaktif, pilih `qna` from the menu and enter your question when prompted.

**Parameters:**
- `question` (string): Pertanyaan untuk mencari jawaban

**Contoh Permintaan:**

```json
{
  "question": "what is artificial intelligence"
}
```

## Detail Kode

Bagian ini menyediakan potongan kode dan referensi untuk implementasi server dan klien.

<details>
<summary>Python</summary>

Lihat [`server.py`](../../../../05-AdvancedTopics/web-search-mcp/server.py) and [`client.py`](../../../../05-AdvancedTopics/web-search-mcp/client.py) untuk detail implementasi lengkap.

```python
# Example snippet from server.py:
import os
import httpx
# ...existing code...
```
</details>

## Konsep Lanjutan dalam Pelajaran Ini

Sebelum mulai membangun, berikut beberapa konsep lanjutan penting yang akan muncul sepanjang bab ini. Memahami ini akan membantu Anda mengikuti dengan lebih mudah, bahkan jika Anda baru mengenalnya:

- **Orkestrasi Multi-alat**: Artinya menjalankan beberapa alat berbeda (seperti pencarian web, berita, produk, dan tanya jawab) dalam satu server MCP. Ini memungkinkan server Anda menangani berbagai tugas, bukan hanya satu.
- **Penanganan Batasan API**: Banyak API eksternal (seperti SerpAPI) membatasi berapa banyak permintaan yang dapat Anda buat dalam waktu tertentu. Kode yang baik memeriksa batas ini dan menanganinya dengan baik, sehingga aplikasi Anda tidak rusak saat mencapai batas.
- **Penguraian Data Terstruktur**: Respons API sering kali kompleks dan berlapis. Konsep ini tentang mengubah respons tersebut menjadi format bersih dan mudah digunakan yang ramah untuk LLM atau program lain.
- **Pemulihan Kesalahan**: Kadang terjadi masalah—mungkin jaringan gagal, atau API tidak mengembalikan yang Anda harapkan. Pemulihan kesalahan berarti kode Anda dapat menangani masalah ini dan tetap memberikan umpan balik yang berguna, bukan crash.
- **Validasi Parameter**: Ini tentang memeriksa bahwa semua input ke alat Anda benar dan aman digunakan. Termasuk menetapkan nilai default dan memastikan tipe data benar, yang membantu mencegah bug dan kebingungan.

Bagian ini akan membantu Anda mendiagnosis dan menyelesaikan masalah umum yang mungkin Anda temui saat bekerja dengan Server MCP Pencarian Web. Jika Anda menemui kesalahan atau perilaku tak terduga saat bekerja dengan Server MCP Pencarian Web, bagian pemecahan masalah ini memberikan solusi untuk masalah paling umum. Tinjau tips ini sebelum mencari bantuan lebih lanjut—sering kali dapat menyelesaikan masalah dengan cepat.

## Pemecahan Masalah

Saat bekerja dengan Server MCP Pencarian Web, Anda mungkin sesekali mengalami masalah—ini normal saat mengembangkan dengan API eksternal dan alat baru. Bagian ini menyediakan solusi praktis untuk masalah paling umum, sehingga Anda dapat kembali ke jalur dengan cepat. Jika Anda menemui kesalahan, mulai dari sini: tips di bawah mengatasi masalah yang paling sering dihadapi pengguna dan sering kali dapat menyelesaikan masalah Anda tanpa bantuan tambahan.

### Masalah Umum

Berikut beberapa masalah paling sering dialami pengguna, beserta penjelasan jelas dan langkah penyelesaiannya:

1. **SERPAPI_KEY hilang di file .env**
   - Jika Anda melihat kesalahan `SERPAPI_KEY environment variable not found`, it means your application can't find the API key needed to access SerpAPI. To fix this, create a file named `.env` in your project root (if it doesn't already exist) and add a line like `SERPAPI_KEY=your_serpapi_key_here`. Make sure to replace `your_serpapi_key_here` with your actual key from the SerpAPI website.

2. **Module not found errors**
   - Errors such as `ModuleNotFoundError: No module named 'httpx'` indicate that a required Python package is missing. This usually happens if you haven't installed all the dependencies. To resolve this, run `pip install -r requirements.txt` in your terminal to install everything your project needs.

3. **Connection issues**
   - If you get an error like `Error during client execution`, it often means the client can't connect to the server, or the server isn't running as expected. Double-check that both the client and server are compatible versions, and that `server.py` is present and running in the correct directory. Restarting both the server and client can also help.

4. **SerpAPI errors**
   - Seeing `Search API returned error status: 401` means your SerpAPI key is missing, incorrect, or expired. Go to your SerpAPI dashboard, verify your key, and update your `.env`, buat file `.env` jika belum ada. Jika kunci Anda benar tapi masih muncul kesalahan ini, periksa apakah kuota tier gratis Anda sudah habis.

### Mode Debug

Secara default, aplikasi hanya mencatat informasi penting. Jika Anda ingin melihat lebih banyak detail tentang apa yang terjadi (misalnya, untuk mendiagnosis masalah rumit), Anda dapat mengaktifkan mode DEBUG. Ini akan menampilkan lebih banyak informasi tentang setiap langkah yang diambil aplikasi.

**Contoh: Output Normal**
```plaintext
2025-06-01 10:15:23,456 - __main__ - INFO - Calling general_search with params: {'query': 'open source LLMs'}
2025-06-01 10:15:24,123 - __main__ - INFO - Successfully called general_search

GENERAL_SEARCH RESULTS:
... (search results here) ...
```

**Contoh: Output DEBUG**
```plaintext
2025-06-01 10:15:23,456 - __main__ - INFO - Calling general_search with params: {'query': 'open source LLMs'}
2025-06-01 10:15:23,457 - httpx - DEBUG - HTTP Request: GET https://serpapi.com/search ...
2025-06-01 10:15:23,458 - httpx - DEBUG - HTTP Response: 200 OK ...
2025-06-01 10:15:24,123 - __main__ - INFO - Successfully called general_search

GENERAL_SEARCH RESULTS:
... (search results here) ...
```

Perhatikan bagaimana mode DEBUG menyertakan baris tambahan tentang permintaan HTTP, respons, dan detail internal lainnya. Ini sangat membantu untuk pemecahan masalah.

Untuk mengaktifkan mode DEBUG, atur level logging ke DEBUG di bagian atas `client.py` or `server.py` Anda:

<details>
<summary>Python</summary>

```python
# At the top of your client.py or server.py
import logging
logging.basicConfig(
    level=logging.DEBUG,  # Change from INFO to DEBUG
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
)
```
</details>

---

## Selanjutnya

- [5.10 Streaming Waktu Nyata](../mcp-realtimestreaming/README.md)

**Penafian**:  
Dokumen ini telah diterjemahkan menggunakan layanan terjemahan AI [Co-op Translator](https://github.com/Azure/co-op-translator). Meskipun kami berusaha untuk akurasi, harap diketahui bahwa terjemahan otomatis mungkin mengandung kesalahan atau ketidakakuratan. Dokumen asli dalam bahasa aslinya harus dianggap sebagai sumber yang otoritatif. Untuk informasi yang penting, disarankan menggunakan terjemahan profesional oleh manusia. Kami tidak bertanggung jawab atas kesalahpahaman atau penafsiran yang salah yang timbul dari penggunaan terjemahan ini.