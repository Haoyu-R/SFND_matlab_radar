# SFND_matlab_radar

## Implementation steps for the 2D CFAR process.
```
for i = Tr+Gr+1:Nr/2-(Gr+Tr)
    for j = Td+Gd+1:Nd-(Gd+Td)
        noise_level = zeros(1,1);
        for p = i-(Tr+Gr): i+Tr+Gr
            for q = j-(Td+Gd): j+Td+Gd
                if(abs(i-p)>Gr || abs(j-q)>Gd)
                    noise_level = noise_level + db2pow(RDM(p, q));
                end
            end
        end
        threshold = offset + pow2db(noise_level/(2*(Td+Gd+1)*2*(Tr+Gr+1)-(Gr*Gd)-1));
        
        if(RDM(i, j)>threshold)
            RDM(i, j) = 1;
        else
            RDM(i, j) = 0;
        end
    end
end
```

## Selection of Training, Guard cells and offset. 
```
%Select the number of Training Cells in both the dimensions.
Tr = 10;
Td = 8;
%Select the number of Guard Cells in both dimensions around the Cell under 
%test (CUT) for accurate estimation
Gr = 4;
Gd = 4;
% offset the threshold by SNR value in dB
offset = 10;
```

## Steps taken to suppress the non-thresholded cells at the edges.
```
RDM(1:Tr+Gr, :) = 0;
RDM(end-Tr-Gr:end, :) = 0;
RDM(:, 1:Td+Gd) = 0;
RDM(:, end-Td-Gd:end) = 0;
```
