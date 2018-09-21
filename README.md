# Sample Rails app to use custom fonts in ImageMagick

If you use ImageMagick and output text in images, you probably want to use custom fonts. This repo contains sample code that I use to manage fonts, test them with ImageMagick locally on my Mac, and use them in production on Heroku.

### To add a new font:

1 Save the font (.ttf or .otf) in .fonts/

2 Run `rake fonts:refresh` to update your rails app configuration

3 Verify that your fonts are listed on the output of #2

Fonts installed in .fonts/ should be automatically detected by ImageMagick running on Heroku. If you aren't running Heroku, ymmv.

### To test your new font locally (on a Mac)

To test fonts on your local machine, ImageMagick needs to know about our installed fonts.

1 Run `rake fonts:mac_install` . This sets up configurations in the ~/.magick/ folder for the current user, which are read by ImageMagick.

2 Ensure our fonts are listed in the output of #1

3 restart your Rails server


### To create pretty pictures with text

I use the [rmagick gem](https://rubygems.org/gems/rmagick/versions/2.16.0), which provides a nicer interface on top of ImageMagick.

    require 'rmagick'
    include Magick

    image_specs = Magick::Image.read("caption:I am pink") do
      self.size = "800x600"
      self.colorspace = RGBColorspace
      self.background_color = "Transparent"
      self.gravity = Magick::CenterGravity
      self.depth = 8
      self.format = 'PNG'
      self.fill = '#ff72ee'
      self.font = 'GotischWeissUNZ1AI'
    end.first
    image_specs.write 'i-am-pink.png'

### Notes:
This repo is being provided for educational purposes only and will not be maintained.

The included [perl script](lib/font_configuration_generator.pl) is run by the rake task. It is sourced directly from ImageMagick at [https://www.imagemagick.org/Usage/scripts/imagick\_type\_gen](https://www.imagemagick.org/Usage/scripts/imagick\_type\_gen). It is strongly recommended that use that script as is, and obtain the latest version from that URL instead of this repo.

The included sample font (Gotisch Weiss) is licensed under the SIL [Open Font License](Open\ Font\ License.txt).
