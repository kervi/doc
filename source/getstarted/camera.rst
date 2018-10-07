=====================
Camera (Raspberry Pi)
=====================

It is very easy to stream the Raspberry Pi camera to the web ui.

.. code-block:: python

    if __name__ == '__main__': # this line is important in windows
        from kervi.application import Application
        
        app = Application()
    
        from kervi.vision.camera import CameraStreamer
        
        #Create a streaming camera server
        CAM1 = CameraStreamer("cam1", "camera 1")
        CAM1.flip_vertical = True
        CAM1.flip_horizontal = True
        
        #link camera as background on default dashboard
        CAM1.link_to_dashboard()

        app.run