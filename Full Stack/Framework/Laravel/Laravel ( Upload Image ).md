**Upload single image:**
```php
    public function store(Request $request)
{
    // 1. Validate (Note: for multiple images, use 'image.*')
    $validated = $request->validate([
        'name' => 'required|string|max:255',
        'product_type' => 'required|string|max:100',
        'price' => 'required|numeric|min:0',
        'description' => 'nullable|string',
        'term_condition_id' => 'nullable|exists:term_conditions,id',
        'coupon_id' => 'nullable|exists:coupons,id',
        'promotion_id' => 'nullable|exists:promotions,id',
        'image' => 'nullable|image|mimes:jpeg,png,jpg,gif|max:2048',
    ]);

    // 2. Generate Serial Number
    $latest = Product::orderBy('id', 'desc')->first();
    $nextNumber = ($latest && preg_match('/^pd(\d+)$/', $latest->serial_number, $matches))
                  ? intval($matches[1]) + 1
                  : 1;
    $serialNumber = 'pd' . str_pad($nextNumber, 5, '0', STR_PAD_LEFT);

    // 3. Handle File Upload
    $imageUrl = null;
    if ($request->hasFile('image')) {
        $image = $request->file('image');
        $directory = 'products/';
        // Create directory
        if (!File::isDirectory(public_path($directory))) {
            File::makeDirectory(public_path($directory), 0777, true, true);
        }

        // Generate unique name
        $imageName = $serialNumber . '_' . Str::random(10) . '.' . $image->getClientOriginalExtension();

        // MOVE THE FILE (Correct variable usage)
        $image->move(public_path($directory), $imageName);

        // Set the path for the database
        $imageUrl = $directory . $imageName;
    }

    // 4. Create Product
    $product = Product::create([
        'serial_number'     => $serialNumber,
        'name'              => $validated['name'],
        'product_type'      => $validated['product_type'],
        'price'             => $validated['price'],
        'description'       => $validated['description'] ?? null,
        'term_condition_id' => $validated['term_condition_id'] ?? null,
        'coupon_id'         => $validated['coupon_id'] ?? null,
        'promotion_id'      => $validated['promotion_id'] ?? null,
        'image'             => $imageUrl, // SAVE THE PATH, NOT THE FILE OBJECT
    ]);

    return response()->json([
        'message' => 'Product created successfully.',
        'data' => $product->load(['promotion', 'coupon', 'termCondition']),
    ], 201);
}
```
**Upload Multiple image**
replace single with this function
```php
public function store(Request $request)
{
    // ... validation and serial number logic ...

    $files = $request->file('image'); // Use file() instead of input()
    $directory = 'products/';
    $imagePaths = []; // 1. Initialize an empty array

    if ($request->hasFile('image')) {
        // Ensure it's treated as an array
        $images = is_array($files) ? $files : [$files];

        foreach ($images as $image) {
            // 2. Generate unique name for THIS specific image
            $imageName = $serialNumber . '_' . \Illuminate\Support\Str::random(10) . '.' . $image->getClientOriginalExtension();
            
            // 3. Move the INDIVIDUAL image (use $image, not $images)
            $image->move(public_path($directory), $imageName);
            
            // 4. Push the path into your array
            $imagePaths[] = $directory . $imageName;
        }
    }


}
```

