const upload = document.getElementById('upload');
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const previewArea = document.getElementById('previewArea');
const kbSize = document.getElementById('kbSize');
const downloadBtn = document.getElementById('download');
const resetBtn = document.getElementById('reset');

const TARGET_W = 600;
const TARGET_H = 800;
const MAX_KB = 25;

// 1. AI Library Load Karein - Background Hataane Ke Liye
import('https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.0.0/dist/index.js').then(module => {
  window.removeBackground = module.removeBackground;
});

upload.addEventListener('change', async e => {
  const file = e.target.files[0];
  if(!file) return;

  previewArea.style.display = 'block';
  kbSize.textContent = 'Processing...';

  try {
    // 2. AI se Background Remove karo
    const imageFile = file;
    const blob = await window.removeBackground(imageFile);
    const img = await createImageBitmap(blob);

    // 3. White Background Fill
    ctx.fillStyle = '#FFFFFF';
    ctx.fillRect(0, 0, TARGET_W, TARGET_H);

    // 4. Bande ko Center me Fit karo 600x800 me
    const scale = Math.min(TARGET_W / img.width, TARGET_H / img.height);
    const w = img.width * scale;
    const h = img.height * scale;
    const x = (TARGET_W - w) / 2;
    const y = 20; // Thora upar se start taake sir na katay
    ctx.drawImage(img, x, y, w, h);

    compressToTargetKB(0.9);

  } catch (err) {
    alert('Background remove nahi hua. Koi saaf picture try karein.');
    console.error(err);
  }
});

function compressToTargetKB(quality){
  canvas.toBlob(blob => {
    let sizeKB = blob.size / 1024;
    kbSize.textContent = sizeKB.toFixed(1) + ' KB';

    if(sizeKB > MAX_KB && quality > 0.3){
      compressToTargetKB(quality - 0.1);
    } else {
      window.finalBlob = blob;
    }
  }, 'image/jpeg', quality);
}

downloadBtn.onclick = () => {
  if(!window.finalBlob) return alert('Pehle picture upload karein');
  const a = document.createElement('a');
  a.href = URL.createObjectURL(window.finalBlob);
  a.download = 'passport_600x800.jpg';
  a.click();
};

resetBtn.onclick = () => {
  ctx.clearRect(0,0,TARGET_W,TARGET_H);
  ctx.fillStyle = '#FFFFFF';
  ctx.fillRect(0,0,TARGET_W,TARGET_H);
  previewArea.style.display = 'none';
  upload.value = '';
  kbSize.textContent = '0 KB';
};
