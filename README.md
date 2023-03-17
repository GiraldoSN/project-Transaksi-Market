# project-Transaksi-Market
Codingan ini bersifat meneruskan data analisisnya aja

public function confirm(Requesr $request)
		{
			$temp = TemPurchasr::all();

			if(count($temp) == 0) {
				// Simpan pesan eror dalam session flash data
				session()->flash('error', 'Tidak dapat melakukan insert ke tabel transaksi karena keranjang josong!');
				// Redirect kembali ke halaman sebelumnya
				return redirect()->back();
			}
			// validasi di tabel temporary
			$totalStock = 0;
			foreach($temp as $purchase){
				$barang = Barang::find($purchase->barang_id);
				$totalStok += $barang->stock;
				$totalStock -= $purchase->qty;
			}
			if($totalStok < 0){
				// Simpan pesan error dalam session flash data
				session()->flash('error', 'Total stok barang tidak mencukupi untuk melakukan transaksi');

				// Redirect kembali ke halaman sebelumnya
				return redirect()->back();
			}
			//
			foreach($temp as $purchase){
				$transaksi = new Transaksi();
				$transaksi->barang_id = $barang_id = $purchase->barang_id;
				$transaksi->kasir_id = $purchase->kasir_id;
				$transaksi->qty = $purchase->qty;
				$transaksi-total = $purchase->total;
				$transaksi->save();
			}
			TempPurchase::truncate();
			//simpan pesan sukses dalam session flash data
			session()->flash('succes', 'Data transaksi berhasil disimpan!');
			repurn redirect('/transaksi-kasir');
		}
