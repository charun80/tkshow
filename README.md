<meta http-equiv="refresh" content="0; url=https://codeberg.org/monilophyta/tkshow" />


# tkshow

Visualize images or image sequences together with graphics command list based overlays

Code repository here: [https://codeberg.org/monilophyta/tkshow](https://codeberg.org/monilophyta/tkshow)


## Dependencies

* Python ≥ 3.4
* [NumPy](https://numpy.org/)
* [Pillow](https://pillow.readthedocs.io/en/stable) (PIL fork)


## Usage Examples

### Initializing [Tkinter](https://wiki.python.org/moin/TkInter) environment

```python
import tkshow as tks

def main():
    """main application code goes here"""

    # ...
    # get image from from somewhere
    img = ...

    # draw some overlays
    gcl = tks.GraphicsCmdList()
    gcl.rectangle( 20, 40, 40, 60, outline="black", fill="blue" )
    gcl.marker( [20,20,40,40], [40,60,40,60], symbol="v", size = 4, fill="red", alpha=0.3 )

    # show everything in tkshow window
    tw = tks.create_window()
    tw.set_image( img )
    tw.set_gcl( gcl )

    # do something else or wait until the window is closed:
    tks.tk_main_thread().join()


if __name__ == "__main__":
    tks.tk_init( main_func=main )
```

### Visualize image forward iterator

```python
import tkshow as tks

def img_iter():
    while True:
        # get image from from somewhere
        img = ...

        # draw some overlays
        gcl = tks.GraphicsCmdList()
        gcl.rectangle( ... )
        gcl.marker( ... )

        yield (img,gcl)

def main():
    """main application code goes here"""
    tks.show_sequence( img_iter )

if __name__ == "__main__":
    tks.tk_init( main_func=main )
```


### Visualize forward/backward image callable

```python
import tkshow as tks

def get_image(idx : int):
    
    # get image with index "idx"
    img = ...

    # draw some overlays for "idx"
    gcl = tks.GraphicsCmdList()
    gcl.rectangle( ... )
    gcl.marker( ... )

    return (img,gcl)

def main():
    """main application code goes here"""
    tks.show_sequence( get_image, max_idx=<number of images>-1 )

if __name__ == "__main__":
    tks.tk_init( main_func=main )
```


## API

### Function ```tk_init( main_func : callable )```

Should be called in the beginning of you program before a tkshow window is required.\
The ```main_func``` shall then serves as main function for your entire application.
```tk_init``` closes all tkshow windows and returns after ```main_func``` has finished.

#### Detailed Explanation:

[Tkinter](https://wiki.python.org/moin/TkInter) always has to run within the main thread.
Therefore  ```tk_init``` creates a new thread which processes the callable ```main_func```. 
The main thread initializes TKinter (by calling ```tk_init```) and enters its GUI mainloop.\
When ```main_func``` quits ```tk_init``` closes TKinter and returns.

### Function ```show_sequence( seq, max_idx = None, min_idx = 0 )```

...

### Class ```GraphicsCmdList```

...
