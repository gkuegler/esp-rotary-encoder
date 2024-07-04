# esp-rotary-encoder
Libraries for quadrature rotary encoders for the esp32 platform.
Written in C.

This library uses the built-in pulse count peripheral to count the number of detents moved.

This libary provides two ways of getting the value from the rotary encoder.
1. a counter that requires it to be polled to see if it's been changed
2. a counter which will call a callback when it changes

# Polling Rotary Encoder
This is the most lightweight option and recommended when using with a gui.
This method doesn't require the need to spin up any tasks or call interupts. The pulse counter keeps an internal count and only cpu resources are used when polling to see account has changed. When using gui's, the display update speed is the limiting factor. It doesn't usually make sense for the program to react faster than the display can update. This is especially true if the user is turning the knob very quickly. The count could increment by multiple values in between frames. It makes much more sense to check the count before each frame is rendered in the display loop.

# Interrupt Rotary Encoder
This is an alternate method using interrupts and FreeRTOS tasks.
And interrupted is generated for every detent of the rotary encoder. The interrupt unblocks a FreeRTOS task (which may immediately run depending upon its priority) to call a callback.
A FreeRTOS is used so that the callback can run outside the context of an interrupt handler will also providing immediate response times if the priorities set above the other tasks.