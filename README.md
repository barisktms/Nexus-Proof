Nexus için kontrat deploylayıp dosylarımı yedekledim.

Bir beklentim yok bir kaç dakikalık işlem diye yapıyorum.

Herhangi bir sunucuda yapabilirsiniz.

Kurulum
# güncelleme
sudo apt update -y && sudo apt upgrade -y
sudo apt install cmake
sudo apt install build-essential
# rustup kurulumu

# 1 seçeneğini seçiyoruz
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# rust kurulumu tamamlandıktan sonra
. "$HOME/.cargo/env"
rustup target add riscv32i-unknown-none-elf
# nexus tool kurulumu

# burası biraz uzun sürer - hatalar görürseniz sorun yok.
cargo install --git https://github.com/nexus-xyz/nexus-zkvm cargo-nexus --tag 'v0.2.3'

# nexus oluşturuyoruz
cargo nexus new nexus-project

# main.rs değiştireceğiz
cd nexus-project
# bu dosyaya giriyoruz
nano ./src/main.rs
# içersindeki kodları silip aşağıdaki bloğu girin
#![no_std]
#![no_main]

fn fib(n: u32) -> u32 {
    match n {
        0 => 0,
        1 => 1,
        _ => fib(n - 1) + fib(n - 2),
    }
}

#[nexus_rt::main]
fn main() {
    let n = 7;
    let result = fib(n);
    assert_eq!(result, 13);
}
# contratı run edelim
cargo nexus run
cargo nexus run -v

# prove etmeseini bekleyelim işlemlerin
cargo nexus prove

# verify işlemini tamamlayalım
cargo nexus verify
Bu dizinde ki nexus-proofdosyamızı kaydedip saklayalım

Network Cli
Screen açıp kodu çalıştıralım. Yeni bir versiyonu varsa ilk önce onu indirecek

screen -S nexus
curl https://cli.nexus.xyz/ | sh
Bir süre bekledikten sonra ctrl A D ile kapatabilirsin. İşlem başarılı olduysa prover-id /root/.nexus/ konumundan alabilirsin.

bu kadardı işlemler.
