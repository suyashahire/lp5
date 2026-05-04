#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>
#include <omp.h>

using namespace std;

struct Point {
    double x, y;
    int label;
};

// Euclidean distance
double distance(Point a, Point b) {
    return sqrt((a.x - b.x)*(a.x - b.x) + (a.y - b.y)*(a.y - b.y));
}

// KNN function using OpenMP 
int knn_classify(vector<Point>& dataset, Point query, int K) {
    vector<pair<double, int>> distances(dataset.size());

    #pragma omp parallel for
    for (int i = 0; i < dataset.size(); i++) {
        distances[i] = { distance(dataset[i], query), dataset[i].label };
    }

    sort(distances.begin(), distances.end());

    int count0 = 0, count1 = 0; 
    for (int i = 0; i < K; i++) {
        if (distances[i].second == 0) 
            count0++;
        else 
            count1++;
    }

    return (count1 > count0) ? 1 : 0;
}

int main() { 
    // Sample dataset
    vector<Point> dataset = {
        {1, 2, 0}, {2, 3, 0}, {3, 1, 0},
        {6, 5, 1}, {7, 7, 1}, {8, 6, 1}
    };

    Point query = {5, 5, -1};
    int K = 3;

    int predicted = knn_classify(dataset, query, K);

    cout << "Query Point: (" << query.x << ", " << query.y << ")\n"; 
    cout << "Predicted Class using KNN (K=" << K << "): " << predicted << endl;

    return 0;
}