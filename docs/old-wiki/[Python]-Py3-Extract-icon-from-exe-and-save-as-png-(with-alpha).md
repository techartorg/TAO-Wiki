```python
!#python3
def extract_icon_from_exe(icon_in_path, icon_name, icon_out_path, out_width = 56, out_height = 56):

    """Given an icon path (exe file) extract it and output at the desired width/height as a png image.
    
    Args:
        icon_in_path (string): path to the exe to extract the icon from
        icon_name (string): name of the icon so we can save it out with the correct name
        icon_out_path (string): final destination (FOLDER) - Gets combined with icon_name for full icon_path
        out_width (int, optional): desired icon width
        out_height (int, optional): desired icon height
    
    Returns:
        string: path to the final icon
    """
    import win32ui
    import win32gui
    import win32con
    import win32api
    from PIL import Image

    ico_x = win32api.GetSystemMetrics(win32con.SM_CXICON)
    ico_y = win32api.GetSystemMetrics(win32con.SM_CYICON)

    large, small = win32gui.ExtractIconEx(icon_in_path,0)
    win32gui.DestroyIcon(small[0])

    hdc = win32ui.CreateDCFromHandle( win32gui.GetDC(0) )
    hbmp = win32ui.CreateBitmap()
    hbmp.CreateCompatibleBitmap( hdc, ico_x, ico_x )
    hdc = hdc.CreateCompatibleDC()

    hdc.SelectObject( hbmp )
    hdc.DrawIcon( (0,0), large[0] )

    bmpstr = hbmp.GetBitmapBits(True)
    icon = Image.frombuffer(
        'RGBA',
        (32,32),
        bmpstr, 'raw', 'BGRA', 0, 1
    )

    full_outpath = os.path.join(icon_out_path, "{}.png".format(icon_name))
    icon.resize((out_width, out_height))
    icon.save(full_outpath)
    #return the final path to the image
    return full_outpath
```