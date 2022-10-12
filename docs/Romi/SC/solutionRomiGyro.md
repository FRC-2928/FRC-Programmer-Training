# Solution for RomiGyro Interface

Have the *RomiGyro* class implement Gyro:

    import edu.wpi.first.wpilibj.interfaces.Gyro;

    public class RomiGyro implements Gyro {
      ...

We are now required to implement all of the methods in the interface that we haven't already implemented.  These can be placed at the end of the file:

    @Override
    public void close() throws Exception {
      if (m_gyroSimDevice != null) {
        m_gyroSimDevice.close();
      }
    }

    @Override
    public void calibrate() {
      // no-op
    }

    @Override
    public double getAngle() {
      return getAngleZ();
    }

    @Override
    public double getRate() {
      return getRateZ();
    }

You'll need to create an object variable `m_gyroSimDevice` placed before the constructor.

    private SimDevice m_gyroSimDevice;

<!-- <span style="float:right">
<a href="romiSubsystems">Back</a></span></h3> -->