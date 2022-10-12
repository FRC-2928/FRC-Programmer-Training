# Solution for Slew Rate Limiter Filter
You may have noticed that the movements of the robot are very sudden.  So much so that the tyres may even skid a little at the start of each motion.  In order to reduce that we can add a SkewRateLimiter filter.  Refer to the FRC [Slew Rate Limiter](https://docs.wpilib.org/en/latest/docs/software/advanced-controls/filters/slew-rate-limiter.html) documentation to learn more about these filters.  In this lab we'll create a slew rate filter to give more control over the speed of the robot.

You'll need a separate filter for the forward and backwards driving and for the turns.  These are defined as member variables in the *Drivetrain* class.  The rate parameter adjusts the speed.  You can increase this if you want the robot to go faster.

        private final SlewRateLimiter m_filter = new SlewRateLimiter(0.5);
        private final SlewRateLimiter m_filter_turn = new SlewRateLimiter(0.5);

We don't want to use this filter unless we're very specific about it so create a new method called `rateLimitedArcadeDrive()` to use the filters

    public void rateLimitedArcadeDrive(double xaxisSpeed, double zaxisRotate) {
        m_diffDrive.arcadeDrive(m_filter.calculate(xaxisSpeed), 
                                m_filter_turn.calculate(zaxisRotate));
    }

Update the *ArcadeDrive* command to use the new `rateLimitedArcadeDrive()` method of the Drivetrain.

    public void execute() {
        m_drivetrain.rateLimitedArcadeDrive(m_xaxisSpeedSupplier.get(), m_zaxisRotateSupplier.get());
    }