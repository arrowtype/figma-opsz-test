# Figma `opsz` test

It seems that Figma’s newly released Variable Fonts support (though great overall) doesn’t yet work on variable fonts which have only an optical size axis.

https://user-images.githubusercontent.com/45946693/167951864-1239a78d-f014-4168-8be5-61a148584b7f.mp4

## Test Approach

To test this, I’ve make a "slice" of Fraunces that only has an opsz axis. 

### Process

1. Download [Fraunces 1.0](https://github.com/undercasetype/Fraunces/releases/tag/1.000) from its GitHub repo
2. Git Clone this repo, `cd` into it in a terminal, and then set up a virtual environment and install fontTools:


```
python3 -m venv venv
source venv/bin/activate
pip install fontTools
```


3. Use the [FontTools Instancer](https://fonttools.readthedocs.io/en/latest/varLib/instancer.html) to slice Fraunces:

```
fonttools varLib.instancer "fonts/Fraunces[SOFT,WONK,opsz,wght].ttf" wght=400 SOFT=100 WONK=1 opsz=9:144 --output "fonts/Fraunces-OpSzOnly.ttf"
```

4. Add a font family name suffix to differentiate it in font menus: 

```
python3 scripts/add-familyname-suffix.py "fonts/Fraunces-OpSzOnly.ttf" -s "OpSz Only" --inplace
```

5. Install font on macOS
6. Open the Figma desktop app, update it, and test `Fraunces OpSz Only` in Figma

The Result: Fraunces, with only an Optical Size axis, doesn’t work in Figma, at least on macOS Monterey, `12.3.1`.

### Process for Roboto Flex

I duplicated this process for Roboto Flex, just to be sure it wasn’t particular to Fraunces.

Slice it:

```
fonttools varLib.instancer "fonts/RobotoFlex-VariableFont_GRAD,XTRA,YOPQ,YTAS,YTDE,YTFI,YTLC,YTUC,opsz,slnt,wdth,wght.ttf" GRAD=drop XTRA=drop YOPQ=drop YTAS=drop YTDE=drop YTFI=drop YTLC=drop YTUC=drop XOPQ=drop opsz=8:144 slnt=drop wdth=drop wght=drop --output "fonts/RobotoFlex-OpSzOnly.ttf"
```

Edit its family name:
```
python3 scripts/add-familyname-suffix.py "fonts/RobotoFlex-OpSzOnly.ttf" -s "OpSz Only" --inplace
```

The Result: it also doesn’t work properly in Figma.

## Motivation

I just released v0.1 of [Kyrios](https://kyrios.arrowtype.com), a (regular weight) variable font which has only an Optical Size axis. It seems that Kyrios isn’t quite working in the newly released Figma variable font feature. :( Sliding the "Optical Size" axis doesn’t have any result. Selecting to "Set optical size automatical" *does* work, however.

Possible issues:
1. Maybe it’s due to Kyrios only having an `opsz` axis, and no others, and Figma not being able to handle this. (After testing, this turns out to be the case.)
2. Maybe it’s some internal problem in Kyrios. However, the problem would also have to be present in both Fraunces and Roboto Flex, which seems somewhat unlikely.

I wanted to test whether the issue was on my end or Figma’s, and based on my testing, it seems to be a problem in Figma. Hopefully it can be resolved soon!
