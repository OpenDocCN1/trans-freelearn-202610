# 7

# Fine-Tuning Object Detection Models 139

Technical requirements 140

Setting anchor sizes and anchor ratios 140

Preprocessing input images 140

Sampling training data and generating the default anchors 143

Generating sizes and ratios hyperparameters 144

Setting pixel means and standard deviations 151

Preparing a data loader 152

Calculating the running means and standard deviations 153

Putting it all together 157

Summary 158

# 8

# Image Data Augmentation Techniques 159

Technical requirements 159

Image augmentation techniques 160

Why image augmentations? 160

What are image augmentations? 160

How to perform image augmentations 161

Detectron2’s image augmentation system 164

Transformation classes 167

Augmentation classes 176

The AugInput class 186

Summary 187

# 9

# Applying Train-Time and Test-Time Image Augmentations 189

Technical requirements 189

The Detectron2 data loader 190

Applying existing image augmentation techniques 191

Developing custom image augmentation techniques 192

Modifying the existing data loader 193

Developing the MixUp image augmentation technique 196

Developing the Mosaic image augmentation technique 200

Applying test-time image augmentation techniques 205

Summary 208