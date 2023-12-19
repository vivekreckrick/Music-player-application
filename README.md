# Music-player-application
import javax.swing.*;
import javax.swing.filechooser.FileNameExtensionFilter;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class MusicPlayerApp extends JFrame {
    private List<String> playlist;
    private int currentTrackIndex;
    private boolean isPlaying;

    private JLabel nowPlayingLabel;
    private JButton playPauseButton;
    private JButton stopButton;
    private JButton nextButton;
    private JButton prevButton;

    public MusicPlayerApp() {
        super("Music Player");
        playlist = new ArrayList<>();
        currentTrackIndex = 0;
        isPlaying = false;

        setupUI();
    }

    private void setupUI() {
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(400, 200);
        setLayout(new BorderLayout());

        nowPlayingLabel = new JLabel("Now Playing: ");
        add(nowPlayingLabel, BorderLayout.NORTH);

        JPanel controlPanel = new JPanel(new FlowLayout());

        playPauseButton = new JButton("Play");
        playPauseButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                if (isPlaying) {
                    pause();
                } else {
                    play();
                }
            }
        });
        controlPanel.add(playPauseButton);

        stopButton = new JButton("Stop");
        stopButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                stop();
            }
        });
        controlPanel.add(stopButton);

        prevButton = new JButton("Previous");
        prevButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                playPrevious();
            }
        });
        controlPanel.add(prevButton);

        nextButton = new JButton("Next");
        nextButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                playNext();
            }
        });
        controlPanel.add(nextButton);

        JButton openButton = new JButton("Open");
        openButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                openFile();
            }
        });
        controlPanel.add(openButton);

        add(controlPanel, BorderLayout.CENTER);
    }

    private void play() {
        if (playlist.size() > 0 && currentTrackIndex < playlist.size()) {
            String filePath = playlist.get(currentTrackIndex);
            try {
                // Add code for audio playback here (e.g., using javax.sound.sampled)
                // For brevity, this example does not include the audio playback code.
                nowPlayingLabel.setText("Now Playing: " + new File(filePath).getName());
                isPlaying = true;
                playPauseButton.setText("Pause");
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    private void pause() {
        // Add code to pause audio playback
        isPlaying = false;
        playPauseButton.setText("Play");
    }

    private void stop() {
        // Add code to stop audio playback
        isPlaying = false;
        playPauseButton.setText("Play");
        nowPlayingLabel.setText("Now Playing: ");
    }

    private void playPrevious() {
        if (playlist.size() > 0) {
            currentTrackIndex = (currentTrackIndex - 1 + playlist.size()) % playlist.size();
            stop();
            play();
        }
    }

    private void playNext() {
        if (playlist.size() > 0) {
            currentTrackIndex = (currentTrackIndex + 1) % playlist.size();
            stop();
            play();
        }
    }

    private void openFile() {
        JFileChooser fileChooser = new JFileChooser();
        FileNameExtensionFilter filter = new FileNameExtensionFilter("Audio Files", "mp3", "wav");
        fileChooser.setFileFilter(filter);

        int result = fileChooser.showOpenDialog(this);
        if (result == JFileChooser.APPROVE_OPTION) {
            File selectedFile = fileChooser.getSelectedFile();
            playlist.add(selectedFile.getAbsolutePath());
            if (!isPlaying) {
                play();
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new MusicPlayerApp().setVisible(true);
            }
        });
    }
}
