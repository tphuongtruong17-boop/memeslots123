# MEMESLOTS — Bitcoin Meme DEX on OP_NET

> Every meme has 100 slots. Claim, takeover, earn 1% of every trade on Bitcoin L1.

---

## Cấu trúc repo

```
memeslots/
├── contracts/                  ← Smart contracts (AssemblyScript → WASM)
│   ├── assembly/contracts/
│   │   ├── MemeFactoryV2.ts    ← Registry tất cả meme tokens
│   │   ├── RevenueSharingV2.ts ← 100 slots, cycle revenue
│   │   └── MemeToken.ts        ← OP_20 token với 1% fee
│   └── package.json
├── scripts/
│   └── deploy-factory.mjs      ← Deploy MemeFactoryV2 lên OP_NET
├── web/
│   ├── index.html              ← Toàn bộ dApp (1 file duy nhất)
│   └── vercel.json             ← Vercel config
└── .github/workflows/
    ├── build.yml               ← Build WASM tự động
    ├── deploy-factory.yml      ← Deploy factory (chạy thủ công)
    └── deploy-web.yml          ← Deploy web lên Vercel tự động
```

---

## Setup — làm 1 lần duy nhất

### Bước 1: Fork repo này

Nhấn **Fork** ở góc trên phải GitHub → fork về account của bạn.

---

### Bước 2: Thêm GitHub Secrets

Vào repo của bạn → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

| Secret name | Giá trị | Bắt buộc? |
|---|---|---|
| `PRIVATE_KEY` | WIF private key của ví deploy | ✅ Bắt buộc |
| `VERCEL_TOKEN` | Token từ vercel.com/account/tokens | Nếu dùng Vercel |
| `VERCEL_ORG_ID` | Team ID từ Vercel | Nếu dùng Vercel |
| `VERCEL_PROJECT_ID` | Project ID từ Vercel | Nếu dùng Vercel |

> ⚠️ **Private key được mã hóa bởi GitHub, không ai nhìn thấy được — kể cả bạn sau khi lưu.**
> Dùng một ví riêng cho deploy, không dùng ví chính.

---

### Bước 3: Lấy WIF private key từ ví

**OP_WALLET:**
1. Mở extension → Settings → Export Private Key
2. Chọn WIF format
3. Copy → paste vào secret `PRIVATE_KEY`

**Testnet BTC faucet (để có tiền deploy):**
https://testnet.opnet.org/faucet

---

### Bước 4: Deploy Factory lên OP_NET

1. Vào tab **Actions** trong repo
2. Chọn workflow **"🚀 Deploy Factory to OP_NET"**
3. Nhấn **"Run workflow"**
4. Chọn `testnet` (hoặc `mainnet`)
5. Nhấn **"Run workflow"** xanh

GitHub Actions sẽ:
- Build WASM contracts
- Deploy MemeFactoryV2 lên OP_NET
- Tự động cập nhật `FACTORY` address trong `web/index.html`
- Commit lại vào repo

---

### Bước 5: Deploy web lên Vercel

**Cách nhanh nhất — không cần Vercel token:**

1. Vào [vercel.com/new](https://vercel.com/new)
2. Import repo GitHub này
3. **Root Directory:** `web`
4. Deploy → xong!

Vercel sẽ tự deploy lại mỗi khi bạn push code.

---

## Workflow hàng ngày

```
Sửa contract → push → GitHub Actions tự build WASM
Muốn deploy factory mới → Actions → Run workflow → chọn network
Sửa web/index.html → push → Vercel tự deploy
```

---

## Secrets cần thiết

```
PRIVATE_KEY = KwDiBf89QgGbjEhKnhXJuH7LrciVrZi3qYjgd9M7rFU78NMTiUEe
              ↑ ví dụ WIF format — thay bằng key thật của bạn
```

---

## Testnet links

- Explorer: https://testnet.opnet.org
- Faucet:   https://testnet.opnet.org/faucet
- Motoswap: https://motoswap.org

---

## License

MIT
