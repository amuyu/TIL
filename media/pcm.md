

# 16bit pcm 데이터 증폭
[Audio: Change Volume of samples in byte array](https://stackoverflow.com/questions/14485873/audio-change-volume-of-samples-in-byte-array)
```java
private byte[] adjustVolume(byte[] audioSamples, float volume) {
        byte[] array = new byte[audioSamples.length];
        for (int i = 0; i < array.length; i+=2) {
            // convert byte pair to int
            short buf1 = audioSamples[i+1];
            short buf2 = audioSamples[i];

            buf1 = (short) ((buf1 & 0xff) << 8);
            buf2 = (short) (buf2 & 0xff);

            short res= (short) (buf1 | buf2);
            res = (short) (res * volume);

            // convert back
            array[i] = (byte) res;
            array[i+1] = (byte) (res >> 8);

        }
        return array;
}
```

```java
// little endian
private byte[] adjustVolume(byte[] audioSamples, double volume) {
    byte[] array = new byte[audioSamples.length];
    for (int i = 0; i < array.length; i+=2) {
        // convert byte pair to int
        int audioSample = (int) ((audioSamples[i+1] & 0xff) << 8) | (audioSamples[i] & 0xff);

        audioSample = (int) (audioSample * volume);

        // convert back
        array[i] = (byte) audioSample;
        array[i+1] = (byte) (audioSample >> 8);

    }
    return array;
}
```
